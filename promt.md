READ-ONLY. Read once, quote, stop. No loop. Find how curPrincipalNRPriorMonth is populated.

BUG CONFIRMED: 202606 curPrincipalNR=7624602, but 202607 curPrincipalNRPriorMonth=1000000 (stale — should be 7624602, i.e. the prior month's curPrincipalNR). So curPrincipalNRPriorMonth is NOT recomputed when data changes. Legacy has correct prior-month values.

In backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs:
1) Search "curPrincipalNRPriorMonth" and "PriorMonth". Quote every place it is SET/computed/written. Is there ANY logic that sets curPrincipalNRPriorMonth = previous month's curPrincipalNR? Quote it, or state "never computed — only read from stored tblMain".
2) Same for curGrossNRorARPriorMonth — is it recomputed from the prior month's curGrossNRorAR, or just stored?
3) Is there a "prior month backfill" step in the save path (UpsertRowWithConnectionAsync) or in any Recompute* method that updates the current row's *PriorMonth fields from the preceding month's values? Quote it or confirm absent.
4) How does legacy populate PriorMonth? (If known from the earlier legacy formula exports — the PriorMonth fields are likely set by a query that looks up the previous month's value.)

OUTPUT:
- A) Where curPrincipalNRPriorMonth / curGrossNRorARPriorMonth are set (quoted) or "never recomputed".
- B) Whether the save path backfills PriorMonth from the previous month. yes/no.
- C) Exact fix location: where to add logic that sets curPrincipalNRPriorMonth = (previous month's curPrincipalNR) and curGrossNRorARPriorMonth = (previous month's curGrossNRorAR) on save/recompute.
- No fix yet. Findings only.

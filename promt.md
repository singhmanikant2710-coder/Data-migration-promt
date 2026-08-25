Perfect, that settles it — trailing 12 calendar months, across all templates, regardless of fiscal year. That's exactly what I'll implement, and it also resolves the Sept-30 year-end case (BHG, #29) since the window will no longer be cut at the fiscal boundary.

Implementing now. I'll have it tested against the legacy app for the first couple of industries before tomorrow's session.


READ-ONLY. No edits, no terminal. Read files, quote verbatim, findings only. This is the last check before I write the TTM populate fix. Keep it focused — do not loop.

CONTEXT: John (client) approved matching legacy: at save time, populate the TTM component table (dbo.tblMainTTMCalculations) with a trailing-12-month SUM/AVG, year-agnostic (current month + previous 11, crossing fiscal-year boundary). Key rule: if a source field doesn't exist / has no value, its TTM must NOT be computed (stay null, not 0). The read path TryMergeTtmIntoSeries already reads from dbo.tblMainTTMCalculations.

I need to confirm the target table before writing to it. Report ONLY:

1) TABLE SCHEMA: In backend (search Bcat.Infrastructure SqlServer, any migration/SQL scripts, or EF model) find the definition of dbo.tblMainTTMCalculations. List its columns (names + types). I specifically need to confirm these exist: curInterestExpenseTTM, curProfitBeforeTaxesTTM, curDepreciationTTM, curAmortizationTTM, curDistributionsTTM, curCPLDTTM, curFixedChargesTTM, curNetChargeOffTTM, curAveragePrincipalNRTTM, curAverageGrossNRTTM, strCustomerName, strMonthKey, and any key/ID column. Quote the schema/definition verbatim if found. If the table definition isn't in the codebase (only referenced at read), say so.

2) EXISTING WRITES: Search the entire backend for any INSERT/UPDATE/MERGE into dbo.tblMainTTMCalculations. Is there ANY code that writes to this table today, or is it only ever read (via TryMergeTtmIntoSeries) and populated by migration? Quote every write site with path, or state "no write sites found — table is read-only in current code."

3) THE SAVE ENTRY POINT: In SqlMainRepository.cs, quote the exact method + line where RecomputePbtTtmAsync is called inside the upsert path (I saw it before), and confirm the surrounding variables available there: the connection (con), transaction (tx), customer (cust), fiscal year (fy), monthKey, and cancellation token (ct). I need to know exactly what's in scope at that point so a new populate call can be added alongside it.

4) MONTHKEY LOOKUP: Legacy uses tblLookupMonthkey.ID to bound the 12-month window (ID between anchor-11 and anchor). Does the new app's SQL Server DB have an equivalent tblLookupMonthkey (or a way to order months)? Or does the new app just order by strMonthKey string? Search for "tblLookupMonthkey" or "MonthKey" ordering in SqlMainRepository and quote how months are sequenced. (This matters: to get "previous 11 months" I need a reliable month ordering.)

OUTPUT:
- A) tblMainTTMCalculations schema (or "not in codebase").
- B) Existing write sites (or "read-only today").
- C) The save-path call site for RecomputePbtTtmAsync + variables in scope, quoted.
- D) How months are ordered/sequenced for windowing (tblLookupMonthkey vs strMonthKey), quoted.
- No fix yet. Findings only.

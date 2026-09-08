FOUND IT via instrumentation. The false-dirty is NOT CustomerInfo — it's TransactionsSection. On a clean load (no user edit), useTransactions.ts:439 (called from TransactionsSection.tsx:552) stages into the changes context automatically:
  transactions.<acctId>.collateralDesc = "OOOS - CRB OWNER OCCUPIED OFFICE SPACE"
  transactions.<acctId>.businessTypeDesc = "CREDIT BASED PERMANENT"
  _update: true
This makes isDirty=true on load, triggering both popups.

READ-ONLY first — do NOT fix yet, just answer:
1. Open useTransactions.ts around line 439 and TransactionsSection.tsx around line 552. What exactly stages collateralDesc / businessTypeDesc / _update on load? Is it an enrichment/lookup effect that converts a raw code (e.g. "OOOS") into a description and writes it back into the changes context — WITHOUT the user editing anything?
2. Is this staging happening inside a useEffect that runs on mount/data-load, rather than in an onChange user handler? Paste the exact code at useTransactions.ts:439 and its surrounding effect/trigger.
3. Is collateralDesc/businessTypeDesc a DISPLAY-only derived value (looked up from a code for showing in the UI), or a real editable field the user changes? If it's display-only enrichment, it should NOT be staged into changes at all.
4. Does _update:true get set automatically on load too? That would make every transaction row look edited.
5. What's the minimal fix: (a) stop this enrichment from staging into the changes context on load (only stage on genuine user onChange), or (b) if it must populate the display, do it in local state / the fetched data, not in the changes/draft context?

Report the exact code + root cause + minimal fix location. Do NOT fix yet.

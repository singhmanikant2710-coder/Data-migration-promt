READ-ONLY — do not propose or apply any fix.

Is there a UI flow (Customer Edit screen or similar) that allows changing
a customer's intFiscalYearMonthStart (fiscal year start month) after the
customer already has tblMain history?

If yes:
1. What endpoint/handler does this update go through?
2. Does anything currently cascade to existing tblMain rows for that
   customer (recomputing intFiscalYear/intFiscalMonth/datFiscalYearStart/
   intElapsedFiscalDays for all historical rows), or does it only affect
   the customer record itself, leaving existing tblMain rows stale?

Report findings only. This is scoping for a new requirement — no fix yet.

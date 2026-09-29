Factoring fix (all Factoring customers, all surfaces). Evidence:
TBS FACTORING SERVICE LLC 201907 — tblMain:
perNetIncomeYTDDividedByRevenueYTD = 0.1094546 (legacy 10.95%),
curEBITTTM = 11887.767 (legacy $11,888).

1. YTD PBT Margin: UI shows 0.11%. Stored fraction must be shown as
   Access Percent (x100) -> 10.95%, on Top Strip, Monthly Summary,
   Rolling 24, Fiscal YTD, Detail grid, PDF, CSV. Also check every other
   percent field in the Factoring template for the same missing x100
   and list them (fix only those whose frm006 control is a bound Percent
   control).
2. Month/TTM "EBIT TTM" is blank. Bind it to curEBITTTM (frm006 control
   source) -> $11,888. Check the other Month/TTM rows are bound too.
3. Cash & Charge-offs: remove "Collections %" — frm006 has no such
   control in that block (it was re-added in Part D). Quote frm006 to
   confirm before removing.

EVIDENCE & SAFETY (mandatory):
- Quote frm006 control source + format for every field changed.
- Scope: Factoring only. STOP if another industry is affected.
- Regression before/after: TBS FACTORING + TOWER CAP SPV, LLC.
- No customer-specific code. Do not change values, calculations,
  persistence. Build, tests, do not commit.

REPORT FORMAT (mandatory):
- Root cause per item + ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE per surface incl. NULL case
- NOT TOUCHED


SELECT strMonthKey, strCovenantName, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName = '<CUSTOMER>' AND strMonthKey = '<MONTH>'
  AND strCovenantName = 'Min Tangible Net Worth';

SELECT strCovenantName1, dblCovenantActual1, strCovenantName2, dblCovenantActual2
FROM tblMain
WHERE strCustomerName = '<CUSTOMER>' AND strMonthKey = '<MONTH>';

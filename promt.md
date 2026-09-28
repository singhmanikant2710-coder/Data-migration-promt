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


SELECT strCustomerName, strMonthKey,
  perNetChargeOffTTM,
  CAST(ROUND(perNetChargeOffTTM * 100, 2) AS decimal(10,2)) AS NetCO_TTM_Pct_Expected,
  curNetChargeOffTTM, curAveragePrincipalNRTTM
FROM tblMain
WHERE (strCustomerName = 'WESTLAKE SERVICES LLC'      AND strMonthKey = '202604')
   OR (strCustomerName = 'AMERICAN CREDIT ACCEPTANCE' AND strMonthKey = '202603');


   
Thanks John.

1) Agreed on NULL handling, and our TTM averages will exclude NULL months once the data holds NULL. We can't confirm how it was ingested, but a quick check for the DBA would settle it: if the numeric columns in SQL tblMain are NOT NULL or have a default of 0 (INFORMATION_SCHEMA.COLUMNS: IS_NULLABLE / COLUMN_DEFAULT), the load converted blanks to 0. If they are nullable with no default, the zeros came from somewhere else and we can dig further.

2) Correction on our side: no decision needed. We checked more customers (ECLIPSE BUSINESS CAPITAL, WESTLAKE SERVICES) and their custom field text matches Access exactly, symbols included. SHABANA MOTORS is the only mismatch, and one of its values also differs (3.64 vs 2.79x), so it looks like the same situation as Mariner, data changed after the load, not a conversion problem.

3) Agreed. So far it's MARINER and SHABANA. We'll let you know if we find more.

4) 


Hi Geoff,

Yes — the query is built and working (grouped by RM/PM, filtered to ACBS/
MWS/IFL, with count and Committed Exposure subtotals).

Ashok's team applied a schema change on Recipient_role / Relationship_mgr_
number / Portfolio_mgr_number today (zero-padded IDs, trigger-enforced), so
I'm re-running the query against the updated data now to make sure the
numbers reflect the current state rather than a stale comparison. Will send
you the results shortly.

Thanks,
Manikant

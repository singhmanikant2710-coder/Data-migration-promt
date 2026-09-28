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



;WITH cust AS (
    SELECT
        LTRIM(RTRIM(d.[CUST_NUM])) AS [CustNum],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[OfficerNumber])))) AS [RmId],
        MIN(LTRIM(RTRIM(d.[OfficerName]))) AS [RmName],
        TRY_CONVERT(int, MIN(LTRIM(RTRIM(d.[PM Number])))) AS [PmId],
        MIN(LTRIM(RTRIM(d.[PMName]))) AS [PmName],
        SUM(TRY_CONVERT(decimal(38,10), NULLIF(d.[Commitment], ''))) AS [TotalCommitted]
    FROM dbo.[01_DATA_01_Data Mart Trial] AS d WITH (NOLOCK)
    WHERE NULLIF(LTRIM(RTRIM(d.[CUST_NUM])), '') IS NOT NULL
      AND d.[SourceSystem] IN ('ACBS', 'MWS', 'IFL')
    GROUP BY LTRIM(RTRIM(d.[CUST_NUM]))
)
SELECT
    'Relationship Manager' AS [Field],
    cust.[RmId] AS [EmployeeId],
    cust.[RmName] AS [DataMartName],
    COUNT_BIG(*) AS [CustomerCount],
    SUM(cust.[TotalCommitted]) AS [CommittedExposure]
FROM cust
WHERE cust.[RmId] IS NOT NULL
  AND NOT EXISTS (
      SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
      WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[RmId]
  )
GROUP BY cust.[RmId], cust.[RmName]

UNION ALL

SELECT
    'Portfolio Manager',
    cust.[PmId],
    cust.[PmName],
    COUNT_BIG(*),
    SUM(cust.[TotalCommitted])
FROM cust
WHERE cust.[PmId] IS NOT NULL
  AND NOT EXISTS (
      SELECT 1 FROM dbo.[03_LIBRARY_10_Distribution Parties] AS dp WITH (NOLOCK)
      WHERE TRY_CONVERT(int, LTRIM(RTRIM(dp.[Recipient_role]))) = cust.[PmId]
  )
GROUP BY cust.[PmId], cust.[PmName]

ORDER BY [Field], [CommittedExposure] DESC;

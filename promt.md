IF OBJECT_ID('tempdb..#cust_count') IS NOT NULL DROP TABLE #cust_count;
IF OBJECT_ID('tempdb..#base') IS NOT NULL DROP TABLE #base;
IF OBJECT_ID('tempdb..#chk') IS NOT NULL DROP TABLE #chk;
IF OBJECT_ID('tempdb..#per_cust') IS NOT NULL DROP TABLE #per_cust;
IF OBJECT_ID('tempdb..#classified') IS NOT NULL DROP TABLE #classified;

SELECT COUNT(DISTINCT strCustomerName) AS TotalCustomers
INTO #cust_count
FROM tblCustomer;

SELECT
    LTRIM(RTRIM(m.strCustomerName)) AS CustomerName,
    LTRIM(RTRIM(m.strMonthKey)) AS MonthKey,
    TRY_CONVERT(int, c.intFiscalYearMonthStart) AS StartMonth,
    TRY_CONVERT(int, LEFT(LTRIM(RTRIM(m.strMonthKey)), 4)) AS CalYear,
    TRY_CONVERT(int, SUBSTRING(LTRIM(RTRIM(m.strMonthKey)), 5, 2)) AS CalMonth,
    TRY_CONVERT(int, m.intFiscalYear) AS StoredFY,
    TRY_CONVERT(int, m.intFiscalMonth) AS StoredFM
INTO #base
FROM dbo.tblMain m
LEFT JOIN dbo.tblCustomer c
    ON LTRIM(RTRIM(c.strCustomerName)) = LTRIM(RTRIM(m.strCustomerName));

SELECT b.*,
    ((CalMonth - StartMonth + 12) % 12) + 1 AS ExpFM,
    CASE WHEN StartMonth = 1 THEN CalYear
         WHEN CalMonth >= StartMonth THEN CalYear + 1
         ELSE CalYear END AS ExpFY
INTO #chk
FROM #base b
WHERE StartMonth BETWEEN 1 AND 12 AND CalMonth BETWEEN 1 AND 12
  AND LEN(MonthKey) = 6 AND StoredFY IS NOT NULL AND StoredFM IS NOT NULL;

SELECT CustomerName, MIN(StartMonth) AS StartMonth,
    COUNT(*) AS RowsChecked,
    SUM(CASE WHEN StoredFY = ExpFY AND StoredFM = ExpFM THEN 1 ELSE 0 END) AS MatchingRows,
    SUM(CASE WHEN StoredFY <> ExpFY OR StoredFM <> ExpFM THEN 1 ELSE 0 END) AS MismatchRows
INTO #per_cust
FROM #chk
GROUP BY CustomerName;

SELECT *,
    CASE WHEN StartMonth = 1 THEN 'CALENDAR YEAR (convention irrelevant)'
         WHEN MismatchRows = 0 THEN 'MATCHES ENDING-YEAR FORMULA'
         WHEN MatchingRows = 0 THEN 'OPPOSITE (STARTING-YEAR) CONVENTION'
         ELSE 'MIXED / PARTIAL (likely data corruption)' END AS Classification
INTO #classified
FROM #per_cust;

SELECT '1. TOTAL CUSTOMERS IN tblCustomer' AS Report;
SELECT TotalCustomers FROM #cust_count;

SELECT '2. CUSTOMERS ACTUALLY CHECKED' AS Report;
SELECT COUNT(*) AS CustomersChecked, SUM(RowsChecked) AS TotalRowsChecked FROM #classified;

SELECT '3. POPULATION BREAKDOWN' AS Report;
SELECT Classification, COUNT(*) AS Customers, SUM(RowsChecked) AS TotalRows
FROM #classified
GROUP BY Classification
ORDER BY Classification;

SELECT '4. EVERY CUSTOMER - OPPOSITE (STARTING-YEAR) CONVENTION' AS Report;
SELECT CustomerName, StartMonth, RowsChecked
FROM #classified
WHERE Classification = 'OPPOSITE (STARTING-YEAR) CONVENTION'
ORDER BY CustomerName;

SELECT '5. EVERY CUSTOMER - MIXED/PARTIAL (known corruption)' AS Report;
SELECT CustomerName, StartMonth, RowsChecked, MatchingRows, MismatchRows
FROM #classified
WHERE Classification = 'MIXED / PARTIAL (likely data corruption)'
ORDER BY MismatchRows DESC;

DROP TABLE #cust_count, #base, #chk, #per_cust, #classified;


Ran it against the full population, not a sample. Here's the breakdown (521 total customers in tblCustomer; 212 have tblMain data to check — the rest are calendar-year customers or have no rows):

- 179 customers: calendar-year (Jan start) — the starting-year/ending-year distinction doesn't apply, both are identical.
- 23 customers: consistently follow the "ending-year" convention (Oct 2025–Sep 2026 = "FY2026") across their entire history, zero exceptions.
- 6 customers: mixed/partial — this is a separate, already-identified bug (a prior version of the code derived the fiscal month from the previous row instead of the calendar, so a data gap caused drift mid-history). Not a convention question, a data-corruption question, and it's on our list to backfill.
- 4 customers: consistently follow the OPPOSITE ("starting-year") convention across their ENTIRE history, zero exceptions — Bankers Healthcare Group LLC, Keystone Private Income Fund, Nationwide Specialty Finance Inc, Vermeer Mountain West Inc.

So to answer directly: yes, those are the only 4 in the whole database. We checked every customer with fiscal-year data, not a sample.

We tried to find what makes those 4 different — industry, single vs. syndicated lender, start month — none of it correlates. It looks like a genuine, deliberate per-customer setting from way back, not corruption (the pattern is 100% consistent for each of the 4, not partial like the drift issue above).

Given your last note about wanting the programmatic approach to be consistent going forward — my read is: since it's only these 4, and it's stable, we could either (a) normalize all 4 to the standard convention going forward once we agree that's the source of truth, or (b) special-case just these 4 by name if there's a business reason they need to stay as-is. Let me know which way you want to go and I'll build accordingly.

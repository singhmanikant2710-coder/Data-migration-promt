SET NOCOUNT ON;
IF OBJECT_ID('tempdb..#cust') IS NOT NULL DROP TABLE #cust;
IF OBJECT_ID('tempdb..#chk') IS NOT NULL DROP TABLE #chk;
IF OBJECT_ID('tempdb..#seq') IS NOT NULL DROP TABLE #seq;

SELECT LTRIM(RTRIM(strCustomerName)) AS CustomerName,
       MIN(TRY_CONVERT(int, intFiscalYearMonthStart)) AS StartMonth
INTO #cust
FROM dbo.tblCustomer
WHERE NULLIF(LTRIM(RTRIM(strCustomerName)), '') IS NOT NULL
GROUP BY LTRIM(RTRIM(strCustomerName));

WITH base AS (
    SELECT LTRIM(RTRIM(m.strCustomerName)) AS CustomerName,
           LTRIM(RTRIM(m.strMonthKey)) AS MonthKey,
           c.StartMonth,
           TRY_CONVERT(int, LEFT(LTRIM(RTRIM(m.strMonthKey)), 4)) AS CalYear,
           TRY_CONVERT(int, SUBSTRING(LTRIM(RTRIM(m.strMonthKey)), 5, 2)) AS CalMonth,
           TRY_CONVERT(int, m.intFiscalYear) AS StoredFY,
           TRY_CONVERT(int, m.intFiscalMonth) AS StoredFM
    FROM dbo.tblMain m
    LEFT JOIN #cust c ON c.CustomerName = LTRIM(RTRIM(m.strCustomerName))
),
flagged AS (
    SELECT b.*,
        CASE WHEN b.StartMonth BETWEEN 1 AND 12
              AND b.CalMonth BETWEEN 1 AND 12
              AND b.CalYear BETWEEN 1900 AND 2999
              AND LEN(b.MonthKey) = 6
              AND b.StoredFY IS NOT NULL AND b.StoredFM IS NOT NULL
        THEN 1 ELSE 0 END AS IsCheckable
    FROM base b
)
SELECT f.*,
    ((f.CalMonth - f.StartMonth + 12) % 12) + 1 AS ExpFM,
    CASE WHEN f.StartMonth = 1 THEN f.CalYear
         WHEN f.CalMonth >= f.StartMonth THEN f.CalYear + 1
         ELSE f.CalYear END AS ExpFY,
    CAST(f.CalYear AS bigint) * 100 + f.CalMonth AS MonthOrdinal
INTO #chk
FROM flagged f
WHERE f.IsCheckable = 1;

SELECT k.*,
    ((k.StoredFM - k.ExpFM) % 12 + 12) % 12 AS FmOffset,
    LAG(k.StoredFM) OVER (PARTITION BY k.CustomerName ORDER BY k.MonthOrdinal) AS PrevStoredFM,
    LAG(k.MonthOrdinal) OVER (PARTITION BY k.CustomerName ORDER BY k.MonthOrdinal) AS PrevOrdinal,
    CASE WHEN k.StoredFM <> k.ExpFM OR k.StoredFY <> k.ExpFY THEN 1 ELSE 0 END AS IsMismatch
INTO #seq
FROM #chk k;

SELECT 'A. TOTALS' AS Report;
SELECT COUNT(*) AS RowsChecked, SUM(IsMismatch) AS Mismatches,
       COUNT(DISTINCT CASE WHEN IsMismatch = 1 THEN CustomerName END) AS CustomersAffected
FROM #seq;

SELECT 'B. CLASSIFICATION' AS Report;
WITH agg AS (
    SELECT CustomerName, MIN(StartMonth) AS StartMonth,
           SUM(IsMismatch) AS Mismatches,
           COUNT(DISTINCT CASE WHEN IsMismatch = 1 THEN FmOffset END) AS DistinctOffsets,
           SUM(CASE WHEN PrevOrdinal IS NOT NULL AND StoredFM = ((PrevStoredFM % 12) + 1) THEN 1 ELSE 0 END) AS ConsecPlus1,
           SUM(CASE WHEN PrevOrdinal IS NOT NULL THEN 1 ELSE 0 END) AS ConsecPairs,
           SUM(CASE WHEN StoredFM NOT BETWEEN 1 AND 12 THEN 1 ELSE 0 END) AS InvalidFM
    FROM #seq GROUP BY CustomerName
)
SELECT StartMonth, CustomerName, Mismatches, DistinctOffsets, ConsecPairs, ConsecPlus1, InvalidFM,
    CASE WHEN InvalidFM > 0 THEN 'REVIEW: FM out of 1-12'
         WHEN DistinctOffsets = 1 AND ConsecPlus1 = ConsecPairs THEN 'CORRUPTION: clean +1 chain'
         WHEN DistinctOffsets = 1 THEN 'CORRUPTION: broken chain'
         ELSE 'REVIEW: multiple offsets' END AS Classification
FROM agg WHERE Mismatches > 0
ORDER BY Classification DESC, StartMonth, CustomerName;

SELECT 'C. SUMMARY BY CLASS' AS Report;
WITH agg AS (
    SELECT CustomerName, SUM(IsMismatch) AS Mismatches,
           COUNT(DISTINCT CASE WHEN IsMismatch = 1 THEN FmOffset END) AS DistinctOffsets,
           SUM(CASE WHEN PrevOrdinal IS NOT NULL AND StoredFM = ((PrevStoredFM % 12) + 1) THEN 1 ELSE 0 END) AS ConsecPlus1,
           SUM(CASE WHEN PrevOrdinal IS NOT NULL THEN 1 ELSE 0 END) AS ConsecPairs,
           SUM(CASE WHEN StoredFM NOT BETWEEN 1 AND 12 THEN 1 ELSE 0 END) AS InvalidFM
    FROM #seq GROUP BY CustomerName
),
cls AS (
    SELECT *, CASE WHEN InvalidFM > 0 THEN 'REVIEW: FM out of 1-12'
         WHEN DistinctOffsets = 1 AND ConsecPlus1 = ConsecPairs THEN 'CORRUPTION: clean +1 chain'
         WHEN DistinctOffsets = 1 THEN 'CORRUPTION: broken chain'
         ELSE 'REVIEW: multiple offsets' END AS Classification
    FROM agg WHERE Mismatches > 0
)
SELECT Classification, COUNT(*) AS Customers, SUM(Mismatches) AS MismatchedRows
FROM cls GROUP BY Classification;

SELECT 'D. NON-SIGNATURE DETAIL' AS Report;
WITH agg AS (
    SELECT CustomerName, COUNT(DISTINCT CASE WHEN IsMismatch = 1 THEN FmOffset END) AS DistinctOffsets,
           SUM(CASE WHEN StoredFM NOT BETWEEN 1 AND 12 THEN 1 ELSE 0 END) AS InvalidFM,
           SUM(IsMismatch) AS Mismatches
    FROM #seq GROUP BY CustomerName
)
SELECT s.StartMonth, s.CustomerName, s.MonthKey, s.StoredFY, s.ExpFY, s.StoredFM, s.ExpFM, s.FmOffset
FROM #seq s JOIN agg a ON a.CustomerName = s.CustomerName
WHERE a.Mismatches > 0 AND (a.DistinctOffsets > 1 OR a.InvalidFM > 0)
ORDER BY s.StartMonth, s.CustomerName, s.MonthKey;

DROP TABLE #seq; DROP TABLE #chk; DROP TABLE #cust;

SELECT TOP (200)
       LTRIM(RTRIM(m.strCustomerName)) AS CustomerName,
       MIN(TRY_CONVERT(int, m.intFiscalYearMonthStart)) AS MainStart_Min,
       MAX(TRY_CONVERT(int, m.intFiscalYearMonthStart)) AS MainStart_Max,
       MIN(TRY_CONVERT(int, c.intFiscalYearMonthStart)) AS CustomerStart,
       COUNT(*) AS RowCount
FROM dbo.tblMain m
JOIN dbo.tblCustomer c ON LTRIM(RTRIM(c.strCustomerName)) = LTRIM(RTRIM(m.strCustomerName))
WHERE TRY_CONVERT(int, m.intFiscalYearMonthStart) IS NOT NULL
GROUP BY LTRIM(RTRIM(m.strCustomerName))
HAVING MIN(TRY_CONVERT(int, m.intFiscalYearMonthStart)) <> MIN(TRY_CONVERT(int, c.intFiscalYearMonthStart))
    OR MIN(TRY_CONVERT(int, m.intFiscalYearMonthStart)) <> MAX(TRY_CONVERT(int, m.intFiscalYearMonthStart))
ORDER BY CustomerName;

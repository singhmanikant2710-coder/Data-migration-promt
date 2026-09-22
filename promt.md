;WITH cust_count AS (
    SELECT COUNT(DISTINCT strCustomerName) AS TotalCustomers FROM tblCustomer
),
base AS (
    SELECT
        LTRIM(RTRIM(m.strCustomerName)) AS CustomerName,
        LTRIM(RTRIM(m.strMonthKey)) AS MonthKey,
        TRY_CONVERT(int, c.intFiscalYearMonthStart) AS StartMonth,
        TRY_CONVERT(int, LEFT(LTRIM(RTRIM(m.strMonthKey)), 4)) AS CalYear,
        TRY_CONVERT(int, SUBSTRING(LTRIM(RTRIM(m.strMonthKey)), 5, 2)) AS CalMonth,
        TRY_CONVERT(int, m.intFiscalYear) AS StoredFY,
        TRY_CONVERT(int, m.intFiscalMonth) AS StoredFM
    FROM dbo.tblMain m
    LEFT JOIN dbo.tblCustomer c
        ON LTRIM(RTRIM(c.strCustomerName)) = LTRIM(RTRIM(m.strCustomerName))
),
chk AS (
    SELECT b.*,
        ((CalMonth - StartMonth + 12) % 12) + 1 AS ExpFM,
        CASE WHEN StartMonth = 1 THEN CalYear
             WHEN CalMonth >= StartMonth THEN CalYear + 1
             ELSE CalYear END AS ExpFY
    FROM base b
    WHERE StartMonth BETWEEN 1 AND 12 AND CalMonth BETWEEN 1 AND 12
      AND LEN(MonthKey) = 6 AND StoredFY IS NOT NULL AND StoredFM IS NOT NULL
),
per_cust AS (
    SELECT CustomerName, MIN(StartMonth) AS StartMonth,
        COUNT(*) AS RowsChecked,
        SUM(CASE WHEN StoredFY = ExpFY AND StoredFM = ExpFM THEN 1 ELSE 0 END) AS MatchingRows,
        SUM(CASE WHEN StoredFY <> ExpFY OR StoredFM <> ExpFM THEN 1 ELSE 0 END) AS MismatchRows
    FROM chk
    GROUP BY CustomerName
),
classified AS (
    SELECT *,
        CASE WHEN StartMonth = 1 THEN 'CALENDAR YEAR (convention irrelevant)'
             WHEN MismatchRows = 0 THEN 'MATCHES ENDING-YEAR FORMULA'
             WHEN MatchingRows = 0 THEN 'OPPOSITE (STARTING-YEAR) CONVENTION'
             ELSE 'MIXED / PARTIAL (likely data corruption)' END AS Classification
    FROM per_cust
)

SELECT '1. TOTAL CUSTOMERS IN tblCustomer' AS Report;
SELECT TotalCustomers FROM cust_count;

SELECT '2. CUSTOMERS ACTUALLY CHECKED (have tblMain rows + valid start month)' AS Report;
SELECT COUNT(*) AS CustomersChecked, SUM(RowsChecked) AS TotalRowsChecked FROM classified;

SELECT '3. POPULATION BREAKDOWN' AS Report;
SELECT Classification, COUNT(*) AS Customers, SUM(RowsChecked) AS TotalRows
FROM classified
GROUP BY Classification
ORDER BY Classification;

SELECT '4. EVERY CUSTOMER IN "OPPOSITE (STARTING-YEAR) CONVENTION" — full, exhaustive list' AS Report;
SELECT CustomerName, StartMonth, RowsChecked
FROM classified
WHERE Classification = 'OPPOSITE (STARTING-YEAR) CONVENTION'
ORDER BY CustomerName;

SELECT '5. EVERY CUSTOMER IN "MIXED/PARTIAL" — for reference, separate known corruption issue' AS Report;
SELECT CustomerName, StartMonth, RowsChecked, MatchingRows, MismatchRows
FROM classified
WHERE Classification = 'MIXED / PARTIAL (likely data corruption)'
ORDER BY MismatchRows DESC;

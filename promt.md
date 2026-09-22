;WITH base AS (
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
    WHERE StartMonth BETWEEN 1 AND 12
      AND CalMonth BETWEEN 1 AND 12
      AND LEN(MonthKey) = 6
      AND StoredFY IS NOT NULL
      AND StoredFM IS NOT NULL
),
per_cust AS (
    SELECT
        CustomerName,
        MIN(StartMonth) AS StartMonth,
        COUNT(*) AS RowsChecked,
        SUM(CASE WHEN StoredFY = ExpFY AND StoredFM = ExpFM THEN 1 ELSE 0 END) AS MatchingRows,
        SUM(CASE WHEN StoredFY <> ExpFY OR StoredFM <> ExpFM THEN 1 ELSE 0 END) AS MismatchRows
    FROM chk
    WHERE StartMonth <> 1   -- calendar-year customers are identical either way, exclude
    GROUP BY CustomerName
),
classified AS (
    SELECT *,
        CASE
            WHEN MismatchRows = 0 THEN 'MATCHES FORMULA'
            WHEN MatchingRows = 0 THEN 'OPPOSITE CONVENTION'
            ELSE 'MIXED / PARTIAL'
        END AS Classification
    FROM per_cust
)
SELECT
    StartMonth,
    Classification,
    COUNT(*) AS Customers,
    SUM(RowsChecked) AS TotalRows
FROM classified
GROUP BY StartMonth, Classification
ORDER BY StartMonth, Classification;


;WITH base AS (
    SELECT
        LTRIM(RTRIM(m.strCustomerName)) AS CustomerName,
        LTRIM(RTRIM(m.strMonthKey)) AS MonthKey,
        TRY_CONVERT(int, c.intFiscalYearMonthStart) AS StartMonth,
        TRY_CONVERT(int, LEFT(LTRIM(RTRIM(m.strMonthKey)), 4)) AS CalYear,
        TRY_CONVERT(int, SUBSTRING(LTRIM(RTRIM(m.strMonthKey)), 5, 2)) AS CalMonth,
        TRY_CONVERT(int, m.intFiscalYear) AS StoredFY,
        TRY_CONVERT(int, m.intFiscalMonth) AS StoredFM,
        c.strIndustry
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
    WHERE StartMonth BETWEEN 1 AND 12
      AND CalMonth BETWEEN 1 AND 12
      AND LEN(MonthKey) = 6
      AND StoredFY IS NOT NULL
      AND StoredFM IS NOT NULL
),
per_cust AS (
    SELECT
        CustomerName,
        MIN(StartMonth) AS StartMonth,
        MIN(strIndustry) AS Industry,
        COUNT(*) AS RowsChecked,
        SUM(CASE WHEN StoredFY = ExpFY AND StoredFM = ExpFM THEN 1 ELSE 0 END) AS MatchingRows,
        SUM(CASE WHEN StoredFY <> ExpFY OR StoredFM <> ExpFM THEN 1 ELSE 0 END) AS MismatchRows
    FROM chk
    WHERE StartMonth <> 1
    GROUP BY CustomerName
)
SELECT StartMonth, CustomerName, Industry, RowsChecked, MatchingRows, MismatchRows
FROM per_cust
WHERE MatchingRows = 0 AND MismatchRows > 0   -- only the "OPPOSITE CONVENTION" ones
ORDER BY StartMonth, CustomerName;

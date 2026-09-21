;WITH base AS (
    SELECT LTRIM(RTRIM(m.strCustomerName)) AS CustomerName,
           LTRIM(RTRIM(m.strMonthKey)) AS MonthKey,
           TRY_CONVERT(int, c.intFiscalYearMonthStart) AS StartMonth,
           TRY_CONVERT(int, LEFT(LTRIM(RTRIM(m.strMonthKey)), 4)) AS CalYear,
           TRY_CONVERT(int, SUBSTRING(LTRIM(RTRIM(m.strMonthKey)), 5, 2)) AS CalMonth,
           TRY_CONVERT(int, m.intFiscalYear) AS StoredFY,
           TRY_CONVERT(int, m.intFiscalMonth) AS StoredFM
    FROM dbo.tblMain m
    JOIN dbo.tblCustomer c ON LTRIM(RTRIM(c.strCustomerName)) = LTRIM(RTRIM(m.strCustomerName))
),
chk AS (
    SELECT *,
        ((CalMonth - StartMonth + 12) % 12) + 1 AS ExpFM,
        CASE WHEN StartMonth = 1 THEN CalYear
             WHEN CalMonth >= StartMonth THEN CalYear + 1
             ELSE CalYear END AS ExpFY
    FROM base
    WHERE StartMonth BETWEEN 1 AND 12 AND CalMonth BETWEEN 1 AND 12
)
SELECT TOP 3 CustomerName, MonthKey, StartMonth, StoredFY, StoredFM
FROM chk
WHERE StartMonth = 6 AND StoredFY = ExpFY AND StoredFM = ExpFM
ORDER BY CustomerName, MonthKey;

SELECT strCustomerName, strMonthKey, intFiscalYear, intFiscalMonth,
       datFiscalYearStart, intFiscalYearMonthStart
FROM tblMain
WHERE (strCustomerName = "CHARTER PIPE LLC" AND strMonthKey = "201801")
   OR (strCustomerName LIKE "CREDIT ACCEPTANCE CORP*" AND strMonthKey IN ("202404","202405","202406"))
   OR (strCustomerName = "BANKERS HEALTHCARE GROUP LLC" AND strMonthKey IN ("202604","202605","202606"))
   OR (strCustomerName = "<STEP-1-SE-MILA-NAAM>" AND strMonthKey = "<STEP-1-SE-MILA-MONTHKEY>")
ORDER BY strCustomerName, strMonthKey;

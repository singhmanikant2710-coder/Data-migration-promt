DECLARE @tol decimal(19,6) = 1.0;
WITH ytd AS (
  SELECT LTRIM(RTRIM(m.strCustomerName)) AS Customer, m.strMonthKey, m.intFiscalMonth,
    CONVERT(decimal(19,6), m.curRevenueOrSalesYTD) AS StoredRevYtd,
    SUM(COALESCE(CONVERT(decimal(19,6), m.curRevenueOrSales), 0)) OVER (
      PARTITION BY LTRIM(RTRIM(m.strCustomerName)), m.intFiscalYear
      ORDER BY m.intFiscalMonth ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS CorrectRevYtd
  FROM dbo.tblMain m
  WHERE m.intFiscalYear > 0 AND m.intFiscalMonth BETWEEN 1 AND 12
)
SELECT * FROM ytd
WHERE ABS(COALESCE(StoredRevYtd,0)) < @tol AND ABS(CorrectRevYtd) >= @tol
ORDER BY Customer, strMonthKey;

SELECT strMonthKey, curRevenueOrSales, curRevenueOrSalesYTD
FROM tblMain
WHERE strCustomerName = "<CUSTOMER>" AND strMonthKey = "<MONTH>";

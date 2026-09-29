DECLARE @tol decimal(19,6) = 1.0;

WITH ytd AS (
  SELECT LTRIM(RTRIM(m.strCustomerName)) AS Customer, m.strMonthKey,
    m.intFiscalYear, m.intFiscalMonth,
    CONVERT(decimal(19,6), m.curRevenueOrSalesYTD) AS StoredRevYtd,
    CONVERT(decimal(19,6), m.curGrossProfitYTD) AS StoredGpYtd,
    SUM(COALESCE(CONVERT(decimal(19,6), m.curRevenueOrSales), 0)) OVER (
      PARTITION BY LTRIM(RTRIM(m.strCustomerName)), m.intFiscalYear
      ORDER BY m.intFiscalMonth ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS CorrectRevYtd,
    SUM(COALESCE(CONVERT(decimal(19,6), m.curGrossProfit), 0)) OVER (
      PARTITION BY LTRIM(RTRIM(m.strCustomerName)), m.intFiscalYear
      ORDER BY m.intFiscalMonth ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS CorrectGpYtd
  FROM dbo.tblMain m
  WHERE m.intFiscalYear > 0 AND m.intFiscalMonth BETWEEN 1 AND 12
)
SELECT Customer, strMonthKey, intFiscalMonth,
  StoredRevYtd, CorrectRevYtd, StoredGpYtd, CorrectGpYtd
FROM ytd
WHERE (ABS(CorrectRevYtd) < @tol AND ABS(COALESCE(StoredRevYtd,0)) >= @tol)
   OR (ABS(CorrectGpYtd)  < @tol AND ABS(COALESCE(StoredGpYtd,0))  >= @tol)
ORDER BY Customer, strMonthKey;


DECLARE @tol decimal(19,6) = 1.0;

WITH ytd AS (
  SELECT
    CONVERT(decimal(19,6), m.curRevenueOrSalesYTD) AS StoredRevYtd,
    CONVERT(decimal(19,6), m.curGrossProfitYTD) AS StoredGpYtd,
    SUM(COALESCE(CONVERT(decimal(19,6), m.curRevenueOrSales), 0)) OVER (
      PARTITION BY LTRIM(RTRIM(m.strCustomerName)), m.intFiscalYear
      ORDER BY m.intFiscalMonth ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS CorrectRevYtd,
    SUM(COALESCE(CONVERT(decimal(19,6), m.curGrossProfit), 0)) OVER (
      PARTITION BY LTRIM(RTRIM(m.strCustomerName)), m.intFiscalYear
      ORDER BY m.intFiscalMonth ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS CorrectGpYtd
  FROM dbo.tblMain m
  WHERE m.intFiscalYear > 0 AND m.intFiscalMonth BETWEEN 1 AND 12
)
SELECT
  SUM(CASE WHEN ABS(COALESCE(StoredRevYtd,0)) < @tol  AND ABS(CorrectRevYtd) < @tol  THEN 1 ELSE 0 END) AS RevYtd_GenuineZero,
  SUM(CASE WHEN ABS(COALESCE(StoredRevYtd,0)) >= @tol AND ABS(CorrectRevYtd) < @tol  THEN 1 ELSE 0 END) AS RevYtd_Damaged,
  SUM(CASE WHEN ABS(COALESCE(StoredRevYtd,0)) < @tol  AND ABS(CorrectRevYtd) >= @tol THEN 1 ELSE 0 END) AS RevYtd_ZeroButShouldNotBe,
  SUM(CASE WHEN ABS(COALESCE(StoredGpYtd,0))  < @tol  AND ABS(CorrectGpYtd)  < @tol  THEN 1 ELSE 0 END) AS GpYtd_GenuineZero,
  SUM(CASE WHEN ABS(COALESCE(StoredGpYtd,0))  >= @tol AND ABS(CorrectGpYtd)  < @tol  THEN 1 ELSE 0 END) AS GpYtd_Damaged,
  SUM(CASE WHEN ABS(COALESCE(StoredGpYtd,0))  < @tol  AND ABS(CorrectGpYtd)  >= @tol THEN 1 ELSE 0 END) AS GpYtd_ZeroButShouldNotBe,
  COUNT(*) AS TotalRows
FROM ytd;

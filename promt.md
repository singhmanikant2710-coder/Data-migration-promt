;WITH s AS (
  SELECT z.strCustomerName, z.strMonthKey, z.intFiscalYear, z.intFiscalMonth,
         z.curProfitBeforeTaxesYTD AS StoredYTD,
         (SELECT SUM(m.curProfitBeforeTaxes) FROM tblMain m
           WHERE m.strCustomerName = z.strCustomerName
             AND m.intFiscalYear = z.intFiscalYear
             AND m.intFiscalMonth <= z.intFiscalMonth) AS SumMonthlyPBT
  FROM tblMain z
  WHERE z.curProfitBeforeTaxesYTD IS NOT NULL AND z.curProfitBeforeTaxesYTD <> 0)
SELECT * FROM s
WHERE SumMonthlyPBT IS NOT NULL AND ABS(StoredYTD - SumMonthlyPBT) > 1
ORDER BY strCustomerName, strMonthKey;

SELECT TOP 10 m.strCustomerName, m.strIndustry, m.strMonthKey,
  m.curProfitBeforeTaxesYTD, t.curProfitBeforeTaxesTTM
FROM tblMain m
JOIN tblMainTTMCalculations t
  ON t.strCustomerName = m.strCustomerName AND t.strMonthKey = m.strMonthKey
WHERE m.curProfitBeforeTaxesYTD = 0
  AND ISNULL(t.curProfitBeforeTaxesTTM, 0) <> 0
ORDER BY CASE WHEN m.strIndustry LIKE 'Manuf%' THEN 0 ELSE 1 END,
         m.strMonthKey DESC;

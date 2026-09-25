SELECT TOP 10 m.strCustomerName, m.strMonthKey, m.curProfitBeforeTaxesYTD
FROM tblMain m
WHERE m.curProfitBeforeTaxesYTD <> 0
  AND m.strIndustry LIKE 'Manuf%'
  AND m.strMonthKey = (SELECT MAX(x.strMonthKey) FROM tblMain x
                       WHERE x.strCustomerName = m.strCustomerName)
ORDER BY m.strMonthKey DESC;

SELECT TOP 3 c.strCustomerName, m.strMonthKey, m.intFiscalYear, m.intFiscalMonth
FROM tblMain m
JOIN tblCustomer c ON LTRIM(RTRIM(m.strCustomerName)) = LTRIM(RTRIM(c.strCustomerName))
WHERE c.intFiscalYearMonthStart = 1
  AND m.strMonthKey >= '202601'
ORDER BY m.strMonthKey DESC;

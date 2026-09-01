-- April start customer (S=4)
SELECT TOP 5 c.strCustomerName, m.strMonthKey, m.intFiscalYear, m.intFiscalMonth, c.intFiscalYearMonthStart
FROM tblMain m
JOIN tblCustomer c ON LTRIM(RTRIM(m.strCustomerName)) = LTRIM(RTRIM(c.strCustomerName))
WHERE c.intFiscalYearMonthStart = 4
ORDER BY m.strMonthKey DESC;

-- October start (BHG jaisा, S=10) — confirm pattern
SELECT TOP 5 c.strCustomerName, m.strMonthKey, m.intFiscalYear, m.intFiscalMonth
FROM tblMain m
JOIN tblCustomer c ON LTRIM(RTRIM(m.strCustomerName)) = LTRIM(RTRIM(c.strCustomerName))
WHERE c.intFiscalYearMonthStart = 10
ORDER BY m.strMonthKey DESC;

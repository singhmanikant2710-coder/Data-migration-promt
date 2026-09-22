SELECT c.intFiscalYearMonthStart AS StartMonth,
       c.strCustomerName,
       MAX(m.strMonthKey) AS LatestMonth
FROM tblCustomer c
JOIN tblMain m ON LTRIM(RTRIM(m.strCustomerName)) = LTRIM(RTRIM(c.strCustomerName))
WHERE c.intFiscalYearMonthStart IN (1, 6, 2, 4, 5, 7, 9, 11)
GROUP BY c.intFiscalYearMonthStart, c.strCustomerName
HAVING MAX(m.strMonthKey) >= '202601'   -- recent data = likely active
ORDER BY StartMonth, LatestMonth DESC;


SELECT strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart, intElapsedFiscalDays
FROM tblMain WHERE strCustomerName='1ST FRANKLIN FINANCIAL CORPORATION'
ORDER BY strMonthKey DESC;

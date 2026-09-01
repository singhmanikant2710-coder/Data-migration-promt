SELECT intFiscalYearMonthStart, COUNT(*) AS customer_count
FROM tblCustomer
WHERE intFiscalYearMonthStart IS NOT NULL
GROUP BY intFiscalYearMonthStart
ORDER BY intFiscalYearMonthStart;

SELECT c.intFiscalYearMonthStart, m.strCustomerName, m.strMonthKey, 
       m.intFiscalYear, m.intFiscalMonth
FROM tblMain AS m
INNER JOIN tblCustomer AS c 
    ON Trim(m.strCustomerName) = Trim(c.strCustomerName)
WHERE Trim(m.strMonthKey) >= '202501'
ORDER BY c.intFiscalYearMonthStart, m.strCustomerName, m.strMonthKey DESC;

SELECT c.intFiscalYearMonthStart, m.strCustomerName, m.strMonthKey, 
       m.intFiscalYear, m.intFiscalMonth
FROM tblMain AS m
INNER JOIN tblCustomer AS c 
    ON Trim(m.strCustomerName) = Trim(c.strCustomerName)
WHERE c.intFiscalYearMonthStart IN (1, 4, 7, 10)
ORDER BY c.intFiscalYearMonthStart, m.strCustomerName, m.strMonthKey DESC;

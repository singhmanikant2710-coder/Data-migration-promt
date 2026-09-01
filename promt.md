SELECT DISTINCT intFiscalYearMonthStart, COUNT(*) AS customer_count
FROM tblCustomer
WHERE intFiscalYearMonthStart IS NOT NULL
GROUP BY intFiscalYearMonthStart
ORDER BY intFiscalYearMonthStart;


SELECT strCustomerName, intFiscalYearMonthStart
FROM tblCustomer
WHERE intFiscalYearMonthStart IS NOT NULL
ORDER BY intFiscalYearMonthStart, strCustomerName;

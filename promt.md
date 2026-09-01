SELECT strCustomerName, intFiscalYearMonthStart
FROM tblCustomer
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%';

SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
ORDER BY strMonthKey DESC;

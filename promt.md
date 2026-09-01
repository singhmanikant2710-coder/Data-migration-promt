SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%VAN ZYVERDEN%'
ORDER BY strMonthKey DESC;

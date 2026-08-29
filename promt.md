SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'BANKERS HEALTHCARE GROUP LLC'
ORDER BY strMonthKey DESC;

-- BHG ke distinct fiscal years
SELECT DISTINCT intFiscalYear
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'BANKERS HEALTHCARE GROUP LLC'
ORDER BY intFiscalYear;

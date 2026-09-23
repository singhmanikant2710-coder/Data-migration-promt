SELECT strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE strCustomerName = "BANKERS HEALTHCARE GROUP LLC"
  AND strMonthKey = "202010"

SELECT TOP 1 strMonthKey, intFiscalYear, intFiscalMonth
FROM tblMain
WHERE strCustomerName = 'BANKERS HEALTHCARE GROUP LLC'
ORDER BY strMonthKey ASC;

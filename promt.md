SELECT strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart, intElapsedFiscalDays
FROM tblMain WHERE strCustomerName='<CUSTOMER NAME>'
ORDER BY strMonthKey DESC;

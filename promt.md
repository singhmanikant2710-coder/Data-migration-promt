SELECT strMonthKey, strCustomerName, intFiscalYear, intFiscalMonth, datFiscalYearStart
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
ORDER BY strMonthKey;

SELECT strMonthKey, strCustomerName, intFiscalYear, intFiscalMonth, datFiscalYearStart, *
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
  AND strMonthKey IN ('202507','202510')
ORDER BY strMonthKey;

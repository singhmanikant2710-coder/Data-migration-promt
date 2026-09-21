SELECT strMonthKey, strCustomerName, intFiscalYear, intFiscalMonth, datFiscalYearStart,
       curTangibleNetWorth, /* baaki jo bhi columns Black Book screen pe dikhte hain */ *
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC'
ORDER BY strMonthKey;

SELECT strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart,
       intElapsedFiscalDays, dblAccountsReceivableTurnDays, curInventoryTurn, perInterestCoverage
FROM tblMain
WHERE strCustomerName='ATHENS PAPER COMPANY INC' AND strMonthKey='202510';

Show me the exact restore SQL you gave earlier for reverting Athens
202510's derived columns after your curl test. I need to run it now —
this row may still be sitting corrupted in BCAT_Dev.

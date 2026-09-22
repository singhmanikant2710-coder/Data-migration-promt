SELECT strMonthKey, intFiscalMonth, intElapsedFiscalDays, dblAccountsReceivableTurnDays,
       curInventoryTurn, perInterestCoverage, dblCovenantActual1
FROM tblMain WHERE strCustomerName='ATHENS PAPER COMPANY INC' AND strMonthKey='202510';

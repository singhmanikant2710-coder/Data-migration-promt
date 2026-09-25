SELECT strMonthKey, intFiscalMonth, intElapsedFiscalDays,
  curAccountsReceivable, curRevenueOrSalesYTD, dblAccountsReceivableTurnDays,
  curInventory, curGrossProfitYTD, perGrossProfitMarginYTD, curInventoryTurn
FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey BETWEEN '202603' AND '202605'
ORDER BY strMonthKey;

SELECT strMonthKey, intFiscalMonth, intElapsedFiscalDays,
  curAccountsReceivable, curRevenueOrSalesYTD, dblAccountsReceivableTurnDays,
  curInventory, curGrossProfitYTD, perGrossProfitMarginYTD, curInventoryTurn
FROM tblMain
WHERE strCustomerName Like "ATHENS PAPER*"
  AND strMonthKey Between "202603" And "202605"
ORDER BY strMonthKey;

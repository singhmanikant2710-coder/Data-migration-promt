SELECT strMonthKey, intFiscalMonth,
  curRevenueOrSales, curRevenueOrSalesYTD,
  SUM(ISNULL(curRevenueOrSales,0)) OVER (ORDER BY strMonthKey ROWS UNBOUNDED PRECEDING) AS ExpectedRevYtd,
  curGrossProfit, curGrossProfitYTD,
  SUM(ISNULL(curGrossProfit,0)) OVER (ORDER BY strMonthKey ROWS UNBOUNDED PRECEDING) AS ExpectedGpYtd,
  dblAccountsReceivableTurnDays, curInventoryTurn
FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%' AND intFiscalYear = 2026
ORDER BY strMonthKey;

SELECT strMonthKey, intFiscalYear, intFiscalMonth,
  curRevenueOrSales, curRevenueOrSalesYTD,
  curProfitBeforeTaxes, curProfitBeforeTaxesYTD
FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey BETWEEN '202501' AND '202512'
ORDER BY strMonthKey;


SELECT strMonthKey, intFiscalYear, intFiscalMonth,
  curRevenueOrSales, curRevenueOrSalesYTD,
  curProfitBeforeTaxes, curProfitBeforeTaxesYTD
FROM tblMain
WHERE strCustomerName Like "ATHENS PAPER*"
  AND strMonthKey Between "202501" And "202512"
ORDER BY strMonthKey;

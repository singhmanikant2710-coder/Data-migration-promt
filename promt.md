SELECT strMonthKey, intFiscalYear, intFiscalMonth, curRevenueOrSales
FROM tblMain WHERE strCustomerName = 'ALAN WIRE COMPANY'
  AND strMonthKey BETWEEN '202505' AND '202603'
ORDER BY strMonthKey;

SELECT strMonthKey, intFiscalYear, intFiscalMonth, curRevenueOrSales
FROM tblMain WHERE strCustomerName = "ALAN WIRE COMPANY"
  AND strMonthKey Between "202505" And "202603"
ORDER BY strMonthKey;

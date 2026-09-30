SELECT c.strCustomerName, COUNT(m.strMonthKey) AS blackbookMonths
FROM tblCustomer c
JOIN tblMain m ON m.strCustomerName = c.strCustomerName
WHERE c.strIndustry IS NULL OR LTRIM(RTRIM(c.strIndustry)) = ''
GROUP BY c.strCustomerName
ORDER BY c.strCustomerName;

SELECT strMonthKey, intFiscalYear, intFiscalMonth, datFiscalYearStart
FROM tblMain
WHERE strCustomerName = 'KEYSTONE PRIVATE INCOME FUND'
  AND (strMonthKey IN ('202104','202110') OR strMonthKey >= '202510')
ORDER BY strMonthKey;

SELECT strMonthKey, intFiscalYear FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey IN ('202104','202110');


SELECT strMonthKey FROM tblMain
WHERE strCustomerName = 'ADIR INTERNATIONAL LLC' AND strMonthKey BETWEEN '202401' AND '202404';

SELECT strMonthKey, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strCovenantName = 'Min Net Income'
  AND strMonthKey BETWEEN '202510' AND '202605'
ORDER BY strMonthKey;


SELECT strMonthKey, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName Like "ATHENS PAPER*"
  AND strCovenantName = "Min Net Income"
  AND strMonthKey Between "202510" And "202605"
ORDER BY strMonthKey;

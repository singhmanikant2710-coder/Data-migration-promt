SELECT strMonthKey, strCovenantActual, strCovenantFormat
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strCovenantName = 'Min Tangible Net Worth'
  AND strMonthKey BETWEEN '202510' AND '202605'
ORDER BY strMonthKey;


SELECT strMonthKey, strCovenantActual, strCovenantFormat
FROM tblMainCovenants
WHERE strCustomerName Like "ATHENS PAPER*"
  AND strCovenantName = "Min Tangible Net Worth"
  AND strMonthKey Between "202510" And "202605"
ORDER BY strMonthKey;


SELECT strMonthKey,
  strCovenantName1, dblCovenantActual1,
  strCovenantName2, dblCovenantActual2,
  strCovenantName3, dblCovenantActual3,
  strCovenantName4, dblCovenantActual4
FROM tblMain
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey BETWEEN '202601' AND '202605'
ORDER BY strMonthKey;

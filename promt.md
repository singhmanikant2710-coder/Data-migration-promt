SELECT strMonthKey, strCovenantActual, strCovenantFormat
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strCovenantName = 'Other 1 (%)'
ORDER BY strMonthKey;

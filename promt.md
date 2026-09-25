SELECT TOP 3 strMonthKey, strCovenantName, strCovenantActual, strCovenantFormat
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strCovenantName LIKE '%Tangible Net Worth%'
ORDER BY strMonthKey DESC;

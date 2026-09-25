SELECT strCustomerName, strMonthKey, strCovenantName,
       strCovenantActual, strCovenantThreshold, strCovenantFormat
FROM tblMainCovenants
WHERE strCovenantName LIKE '%Tangible Net Worth%'
  AND strCovenantFormat LIKE '%[%]%'
ORDER BY strCustomerName, strMonthKey;

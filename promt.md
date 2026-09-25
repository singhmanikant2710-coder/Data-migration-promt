SELECT 'tblMainCovenants' AS src, strCustomerName, strMonthKey, strCovenantName,
       strCovenantActual AS val, intCovenantOrder
FROM tblMainCovenants
WHERE REPLACE(REPLACE(strCovenantActual, ',', ''), '$', '') LIKE '4346919%'
UNION ALL
SELECT 'tblMain', strCustomerName, strMonthKey, strCovenantName1,
       CONVERT(varchar(50), dblCovenantActual1), NULL
FROM tblMain
WHERE dblCovenantActual1 BETWEEN 43469196 AND 43469197
   OR dblCovenantActual2 BETWEEN 43469196 AND 43469197
   OR dblCovenantActual1Formatted LIKE '%43,469,19%'
   OR dblCovenantActual2Formatted LIKE '%43,469,19%';

   SELECT strMonthKey, strCovenantName, strCovenantActual, intCovenantOrder
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey IN ('202604','202605')
ORDER BY strMonthKey, intCovenantOrder;

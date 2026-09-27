SELECT DISTINCT strCovenantName, strCovenantReported, intCovenantOrder
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%'
  AND strMonthKey BETWEEN '202410' AND '202603';

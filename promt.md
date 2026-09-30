SELECT strMonthKey, strCovenantName, intCovenantOrder, strCovenantReported
FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202604'
ORDER BY intCovenantOrder;

SELECT strCovenantReported, COUNT(*) AS rows_
FROM tblMainCovenants
GROUP BY strCovenantReported
ORDER BY rows_ DESC;

SELECT * FROM tblLookupComboBox
WHERE strComboBoxName LIKE '%Reported%';

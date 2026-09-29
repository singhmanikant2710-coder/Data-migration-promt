SELECT strCovenantName, strCovenantActual FROM tblMainCovenants
WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202604';

SELECT strCovenantName1, dblCovenantActual1, strCovenantName2, dblCovenantActual2,
       strCustomField1, strCustomField2
FROM tblMain WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202604';

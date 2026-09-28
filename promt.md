SELECT strMonthKey, curAveragePrincipalNRTTM, curTotalLiabilities, curTotalAdjustedLiabilities
FROM tblMain
WHERE strCustomerName = "LEON'S AUTO SALES, INC" AND strMonthKey = "202604";


SELECT strCovenantName, intCovenantOrder, strCovenantFormat, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName = 'LEON''S AUTO SALES, INC' AND strMonthKey = '202604'
ORDER BY intCovenantOrder;

SELECT strCustomField1, strCustomField2, strCustomField3, strCustomField4,
       strCustomField5, strCustomField6
FROM tblMain
WHERE strCustomerName = 'LEON''S AUTO SALES, INC' AND strMonthKey = '202604';

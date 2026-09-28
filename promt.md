SELECT strMonthKey, strCustomField1, strCustomField2, strCustomField3
FROM tblMain
WHERE strCustomerName = 'ECLIPSE BUSINESS CAPITAL SPV LLC' AND strMonthKey = '202604';

SELECT curCashCollections, curNetChargeOff, curNetChargeOffYTD,
       curNetChargeOffTTM, curDiscountDividedByReserve, perReserveCoverage
FROM tblMain
WHERE strCustomerName = "AMERICAN CREDIT ACCEPTANCE" AND strMonthKey = "202603";

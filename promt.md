
SELECT strMonthKey, curCashCollections, curPrincipalNRPriorMonth, perCashCollections
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName))='AMERICAN CREDIT ACCEPTANCE' AND strMonthKey='202608';

SELECT curCashCollections, curPrincipalNRPriorMonth, perCashCollections
FROM tblMain WHERE Trim(strCustomerName)='AMERICAN CREDIT ACCEPTANCE' AND strMonthKey='202608';

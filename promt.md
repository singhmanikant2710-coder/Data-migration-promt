SELECT strMonthKey, perCashCollections, curCashCollections, curPrincipalNRPriorMonth
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey = '202607';

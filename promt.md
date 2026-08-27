SELECT strMonthKey, curPrincipalNR, curPrincipalNRPriorMonth,
       curCashCollections, perCashCollections
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName))='AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey IN ('202607','202606')
ORDER BY strMonthKey DESC;

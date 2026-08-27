SELECT strMonthKey,
       curCashCollections,
       curPrincipalNRPriorMonth,
       curGrossNRorARPriorMonth,
       '[' + ISNULL(strPrincipalOrGrossCalculationSelectionCashCollection,'NULL') + ']' AS sel,
       perCashCollections
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName))='AMERICAN CREDIT ACCEPTANCE' 
  AND strMonthKey='202607';

SELECT strMonthKey,
       curCashCollections,
       curPrincipalNRPriorMonth,
       curGrossNRorARPriorMonth,
       '[' + ISNULL(strPrincipalOrGrossCalculationSelectionCashCollection,'NULL') + ']' AS sel_cash,
       perCashCollections
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName))='AMERICAN CREDIT ACCEPTANCE' 
  AND strMonthKey='202607';

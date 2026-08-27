SELECT strMonthKey,
       curCashCollections,
       curPrincipalNRPriorMonth,
       curGrossNRorARPriorMonth,
       perCashCollections,
       strPrincipalOrGrossCalculationSelectionCashCollection
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName))='1ST FRANKLIN FINANCIAL CORPORATION'
  AND strMonthKey='202605';


  SELECT curCashCollections, curPrincipalNRPriorMonth, curGrossNRorARPriorMonth,
       perCashCollections, strPrincipalOrGrossCalculationSelectionCashCollection
FROM tblMain
WHERE Trim(strCustomerName)='1ST FRANKLIN FINANCIAL CORPORATION' AND strMonthKey='202605';

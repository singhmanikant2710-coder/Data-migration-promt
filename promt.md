SELECT strMonthKey, curCashCollections, curPrincipalNRPriorMonth, curGrossNRorARPriorMonth,
       strPrincipalOrGrossCalculationSelectionCashCollection
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = '1ST FRANKLIN FINANCIAL CORPORATION'
  AND strMonthKey = '202609';   -- ya jo current select hai

SELECT TOP 8 strMonthKey, intFiscalYear, 
       curProfitBeforeTaxes, curInterestExpense, curPrincipalNR, curCashCollections
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'BANKERS HEALTHCARE GROUP LLC'
ORDER BY strMonthKey DESC;

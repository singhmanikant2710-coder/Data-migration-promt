SELECT strMonthKey, strCustomerName,
       curInterestExpenseTTM, curProfitBeforeTaxesTTM,
       curNetChargeOffTTM, curAveragePrincipalNRTTM,
       datCalculationRun
FROM dbo.tblMainTTMCalculations
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
ORDER BY strMonthKey DESC;

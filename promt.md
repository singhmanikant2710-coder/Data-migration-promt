SELECT strMonthKey, curInterestExpenseTTM, curProfitBeforeTaxesTTM,
       curAverageGrossNRTTM, datCalculationRun
FROM dbo.tblMainTTMCalculations
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
ORDER BY strMonthKey DESC;

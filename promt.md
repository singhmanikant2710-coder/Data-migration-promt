SELECT TOP 10 strCustomerName, strMonthKey,
  curInterestExpense, curCPLTD, curDistributions,
  curPrincipalNR, curIneligibles, curRevenueOrSalesYTD,
  (CASE WHEN ISNULL(curInterestExpense,0) <> 0 THEN 1 ELSE 0 END
 + CASE WHEN ISNULL(curCPLTD,0) <> 0 THEN 1 ELSE 0 END
 + CASE WHEN ISNULL(curDistributions,0) <> 0 THEN 1 ELSE 0 END
 + CASE WHEN ISNULL(curPrincipalNR,0) <> 0 THEN 1 ELSE 0 END
 + CASE WHEN ISNULL(curRevenueOrSalesYTD,0) <> 0 THEN 1 ELSE 0 END) AS score
FROM tblMain
WHERE ISNULL(curInterestExpense,0) <> 0
ORDER BY score DESC, strMonthKey DESC;

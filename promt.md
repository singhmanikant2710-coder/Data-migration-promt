SELECT TOP 30
       strCustomerName, strMonthKey,
       perCashCollections,
       per60DPD,
       perNetChargeOffTTM,
       perInterestCoverageTTM,
       perGrossProfitMargin,
       perReserveCoverage
FROM tblMain
WHERE (perCashCollections <> 0 OR per60DPD <> 0 OR perNetChargeOffTTM <> 0)
ORDER BY strMonthKey DESC, strCustomerName;


SELECT TOP 30
       strCustomerName, strMonthKey,
       perCashCollections,
       per60DPD,
       perNetChargeOffTTM,
       perInterestCoverageTTM,
       perGrossProfitMargin,
       perReserveCoverage
FROM tblMain
WHERE (perCashCollections <> 0 OR per60DPD <> 0 OR perNetChargeOffTTM <> 0)
ORDER BY strMonthKey DESC, strCustomerName;

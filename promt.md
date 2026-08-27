SELECT strMonthKey,
       perCashCollections,
       per60DPD,
       perNetChargeOffTTM,
       perDebtDivTangibleNetWorth,
       perDiscountDividedByReserve,
       perReserveCoverage,
       perGrossProfitMargin,
       perGrossProfitMarginYTD,
       perInterestCoverageTTM,
       perCollateralAvailability,
       perAvailabilityPercent
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName))='AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey='202608';


  SELECT strMonthKey,
       perCashCollections,
       per60DPD,
       perNetChargeOffTTM,
       perDebtDivTangibleNetWorth,
       perDiscountDividedByReserve,
       perReserveCoverage,
       perGrossProfitMargin,
       perGrossProfitMarginYTD,
       perInterestCoverageTTM,
       perCollateralAvailability
FROM tblMain
WHERE Trim(strCustomerName)='AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey='202608';

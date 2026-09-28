DECLARE @c nvarchar(200) = 'MIDDLE GEORGIA MANAGEMENT SERVICES INC';
DECLARE @mk varchar(6) = '202011';

-- 1. Min EBITDA/Interest
SELECT strMonthKey, strCovenantName, intCovenantOrder, strCovenantFormat, strCovenantActual
FROM tblMainCovenants
WHERE strCustomerName = @c AND strMonthKey = @mk
ORDER BY intCovenantOrder;

SELECT strMonthKey,
  strCovenantName1, dblCovenantActual1, strCovenantName2, dblCovenantActual2,
  strCovenantName3, dblCovenantActual3, strCovenantName4, dblCovenantActual4
FROM tblMain WHERE strCustomerName = @c AND strMonthKey = @mk;

-- 2. Net C/O TTM % + Loan Loss Reserves $ / %
SELECT strMonthKey,
  perNetChargeOffTTM, curNetChargeOffTTM,
  curAveragePrincipalNRTTM, curAverageGrossNRTTM,
  strPrincipalOrGrossCalculationSelectionNetChargeOff,
  curDiscountDividedByReserve AS LoanLossReserve_Dollar,
  perDiscountDividedByReserve AS LoanLossReserve_Pct
FROM tblMain WHERE strCustomerName = @c AND strMonthKey = @mk;

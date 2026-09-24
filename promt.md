Pre-existing changes in SqlMainRepository.cs ~2127 and manufacturing.ts
are mine (Hunk A/B). Keep them.

Batch 1 follow-up: also remove the Related Party fallback in
perDebtDivTangibleNetWorth adjExpr (4443-4447) so it uses
(TotalLiabilities - SubordinatedDebt) - OtherB, same as B6.
Keep the unreachable IS NULL branches — harmless.

Show me lines 4509-4528 verbatim (perInterestCoverage/TTM numerator)
— your report text is garbled there. It must be PBT + InterestExpense.

Batch 5: APPROVED with one change — do NOT use the WINDOW clause.
Inline OVER (PARTITION BY ... ORDER BY ... ROWS BETWEEN 11 PRECEDING
AND CURRENT ROW) on all nine aggregates, same as the MERGE CTE at
3733-3741. Keep RecomputePbtTtmAsync (year filter removed) and the
call at 1264. Apply, build, run unit tests, report.


SELECT strCustomerName, strMonthKey,
  dblFixedChargeCoverage,
  CAST(ISNULL(curProfitBeforeTaxes,0)+ISNULL(curInterestExpense,0)+ISNULL(curDepreciation,0)
       +ISNULL(curAmortization,0)-ISNULL(curDistributions,0) AS float)
   / NULLIF(CAST(ISNULL(curCPLTD,0)+ISNULL(curInterestExpense,0) AS float),0) AS expFCC,
  perInterestCoverage,
  CAST(ISNULL(curProfitBeforeTaxes,0)+ISNULL(curInterestExpense,0) AS float)
   / NULLIF(CAST(curInterestExpense AS float),0) AS expIntCov,
  curFixedChargesTTM, ISNULL(curCPLTDTTM,0)+ISNULL(curInterestExpenseTTM,0) AS expFixedChgTTM,
  perIneligiblePercent, CAST(curIneligibles AS float)/NULLIF(CAST(curPrincipalNR AS float),0) AS expInelPct,
  perNetIncomeYTDDividedByRevenueYTD,
  CAST(curProfitBeforeTaxesYTD AS float)/NULLIF(CAST(curRevenueOrSalesYTD AS float),0) AS expNIRev,
  curTotalAdjustedLiabilities,
  ISNULL(curTotalLiabilities,0)-ISNULL(curSubordinatedDebt,0)-ISNULL(curOtherB,0) AS expTAL,
  perReserveCoverage, perDiscountDividedByReserve/NULLIF(perNetChargeOffTTM,0) AS expResCov
FROM tblMain
WHERE strCustomerName = '<CUSTOMER>' AND strMonthKey = '<YYYYMM>';



SELECT strMonthKey, curProfitBeforeTaxesTTM
FROM tblMain WHERE strCustomerName = 'Athens Paper'
  AND strMonthKey BETWEEN '202410' AND '202605' ORDER BY strMonthKey;

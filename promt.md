DECLARE @c nvarchar(200) = 'TBS FACTORING SERVICE LLC';
DECLARE @mk varchar(6) = (SELECT MAX(strMonthKey) FROM tblMain WHERE strCustomerName = @c);

SELECT strMonthKey,

  -- YTD PBT Margin = PBT YTD / Revenue YTD
  curProfitBeforeTaxesYTD, curRevenueOrSalesYTD,
  perNetIncomeYTDDividedByRevenueYTD AS YTD_PBT_Margin_Stored,
  CASE WHEN ISNULL(curRevenueOrSalesYTD,0) = 0 THEN 0
       ELSE curProfitBeforeTaxesYTD / curRevenueOrSalesYTD END AS YTD_PBT_Margin_Calc,

  -- EBIT TTM = PBT TTM + Interest Expense TTM
  curProfitBeforeTaxesTTM, curInterestExpenseTTM,
  curEBITTTM AS EBIT_TTM_Stored,
  ISNULL(curProfitBeforeTaxesTTM,0) + ISNULL(curInterestExpenseTTM,0) AS EBIT_TTM_Calc,

  -- Collections % (Principal N/R ya Gross N/R prior month ke basis pe)
  strPrincipalOrGrossCalculationSelectionCashCollection AS CollectionBasis,
  curCashCollections, curPrincipalNRPriorMonth, curGrossNRorARPriorMonth,
  perCashCollections AS Collections_Pct_Stored,
  CASE WHEN strPrincipalOrGrossCalculationSelectionCashCollection = 'Principal N/R'
       THEN CASE WHEN ISNULL(curPrincipalNRPriorMonth,0) = 0 THEN 0
                 ELSE ROUND(curCashCollections,0) / ROUND(curPrincipalNRPriorMonth,0) END
       ELSE CASE WHEN ISNULL(curGrossNRorARPriorMonth,0) = 0 THEN 0
                 ELSE ROUND(curCashCollections,0) / ROUND(curGrossNRorARPriorMonth,0) END
  END AS Collections_Pct_Calc

FROM tblMain
WHERE strCustomerName = @c AND strMonthKey = @mk;

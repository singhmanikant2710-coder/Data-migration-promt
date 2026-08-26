SINGLE-FILE, ADD-ONLY, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs. Do NOT remove, rename, or modify any existing method, call, or line. Do NOT refactor. You may ONLY ADD a new method and ADD one new call. Show me the unified diff BEFORE applying. Do not run terminal/build. If anything requires changing existing code, STOP and ask instead of doing it.

GOAL (client-approved): At save time, populate dbo.tblMainTTMCalculations for the saved month with trailing-12-month TTM values, matching the legacy Access query qry0105UpdateTTMAppend12 exactly. TTM = trailing 12 calendar months, YEAR-AGNOSTIC (current month + previous 11, crossing fiscal-year boundaries — NO fiscal year filter). Where a source field has no value, its TTM stays NULL (do not force 0). The existing read path TryMergeTtmIntoSeries already reads this table, so once populated the displays fill in.

CONFIRMED SCHEMA of dbo.tblMainTTMCalculations:
- Primary key (unique): strCustomerName + strMonthKey  (composite)
- Columns: strMonthKey (nvarchar, NOT NULL), strCustomerName (nvarchar, NOT NULL), MaxID (int null), curProfitBeforeTaxesTTM, curInterestExpenseTTM, curDepreciationTTM, curAmortizationTTM, curDistributionsTTM, curCPLTDTTM, curFixedChargesTTM, curFixedChargeCoverageTTM, curAveragePrincipalDividedByNRTTM, curNetChargeOffTTM, curAveragePrincipalNRTTM, curAverageGrossNRTTM (all money, nullable), datCalculationRun (datetime2, null)

EXACT CHANGES:

1) ADD a new private static async method named RecomputeTtmCalculationsAsync, placed immediately AFTER the existing RecomputePbtTtmAsync method. Model its STRUCTURE on RecomputePbtTtmAsync (same signature shape: SqlConnection con, SqlTransaction tx, string cust, CancellationToken ct — NOTE: NO fiscalYear parameter, because this is year-agnostic; drop the fy param and any intFiscalYear filtering). It must:

   a) Compute a per-month trailing-12 window over dbo.tblMain, ordered by strMonthKey ascending, using:
      SUM(...) OVER (PARTITION BY LTRIM(RTRIM(strCustomerName)) ORDER BY LTRIM(RTRIM(strMonthKey)) ROWS BETWEEN 11 PRECEDING AND CURRENT ROW)
      for these SUM columns:
        curInterestExpenseTTM   = SUM(curInterestExpense)
        curProfitBeforeTaxesTTM = SUM(curProfitBeforeTaxes)
        curDepreciationTTM      = SUM(curDepreciation)
        curAmortizationTTM      = SUM(curAmortization)
        curDistributionsTTM     = SUM(curDistributions)
        curCPLTDTTM             = SUM(curCPLD)
        curNetChargeOffTTM      = SUM(curNetChargeOff)
      and AVG(...) OVER (same window) for:
        curAveragePrincipalNRTTM = AVG(curPrincipalNR)
        curAverageGrossNRTTM     = AVG(curAverageGrossNR)
      (These match the legacy qry0105UpdateTTMAppend12 field mapping.)

   b) Scope the computation to the given customer only (WHERE LTRIM(RTRIM(strCustomerName)) = LTRIM(RTRIM(@cust))). Do NOT filter by fiscal year. The window naturally uses the customer's full month history so the trailing 12 crosses year boundaries.

   c) MERGE the result into dbo.tblMainTTMCalculations ON (strCustomerName, strMonthKey):
      - WHEN MATCHED: UPDATE the TTM columns above + set datCalculationRun = SYSDATETIME()
      - WHEN NOT MATCHED: INSERT strCustomerName, strMonthKey, the TTM columns above, and datCalculationRun = SYSDATETIME()
      - Do NOT touch columns you are not computing (curFixedChargesTTM, curFixedChargeCoverageTTM, curAveragePrincipalDividedByNRTTM, MaxID) — leave them as-is on UPDATE and NULL on INSERT. Do not delete any rows.

   d) Guard for table/column existence the same defensive way RecomputePbtTtmAsync guards (INFORMATION_SCHEMA probe); if the table or a source column is missing, return without throwing (fail-open). Use AddParam for @cust exactly as the existing methods do. Wrap in the same fail-open style.

   e) Because a source column with no data yields SUM = NULL (SQL SUM of all-NULLs is NULL), the "don't compute when value doesn't exist" rule is satisfied naturally — do not add COALESCE(...,0) around the SUM/AVG for these TTM outputs (we WANT NULL when absent, not 0).

2) ADD the call in the same save path, immediately AFTER the existing RecomputePbtTtmAsync call block, using the same fail-open try/catch and the in-scope variables (con, tx, cust, ct — no fy):

   // Recompute all trailing-12-month TTM components (year-agnostic) into tblMainTTMCalculations after persist
   try
   {
       await RecomputeTtmCalculationsAsync(con, tx, cust, ct).ConfigureAwait(false);
   }
   catch { /* fail-open on TTM recompute */ }

DO NOT:
- Do NOT modify or remove RecomputePbtTtmAsync, RecomputeOtherYtdsAsync, TryMergeTtmIntoSeries, or any existing method/call.
- Do NOT change the Access repository.
- Do NOT add COALESCE(...,0) on the TTM SUM/AVG outputs (NULL-when-absent is intended).
- Do NOT alter existing columns MaxID / curFixedChargesTTM / curFixedChargeCoverageTTM / curAveragePrincipalDividedByNRTTM.

VERIFY BEFORE SHOWING DIFF (report, don't force):
a) Confirm the source monthly columns exist in tblMain: curInterestExpense, curProfitBeforeTaxes, curDepreciation, curAmortization, curDistributions, curCPLD, curNetChargeOff, curPrincipalNR, curAverageGrossNR. If any differ in name, STOP and report the actual name — do not guess.
b) Confirm the new call site has con, tx, cust, ct in scope (same place RecomputePbtTtmAsync is called).
c) Confirm strMonthKey is YYYYMM format so string ORDER BY = chronological (needed for the 12-row window). If format differs, STOP and report.

OUTPUT: the unified diff (new method + new call) and the results of checks a/b/c. Apply nothing until I confirm.

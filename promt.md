-- ====== 1. Cash Collections % ke liye — prior month fields + selections ======
SELECT strMonthKey,
       curPrincipalNRPriorMonth, curGrossNRorARPriorMonth,
       curGrossNRorAR, curPrincipalNR,
       strPrincipalOrGrossCalculationSelectionCashCollection,
       strPrincipalOrGrossCalculationSelectionNetChargeOff,
       strPrincipalOrGrossCalculationSelectionper60DPD
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey = '202607';

-- ====== 2. YTD Net C/O — kya populate hua tblMain mein ======
SELECT strMonthKey, curNetChargeOff, curNetChargeOffYTD,
       curRevenueOrSalesYTD, curProfitBeforeTaxesYTD
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey IN ('202607','202606','202605')
ORDER BY strMonthKey DESC;

-- ====== 3. TTM Net C/O % ke liye — components (tblMainTTMCalculations) ======
SELECT strMonthKey, curNetChargeOffTTM, curAveragePrincipalNRTTM, curAverageGrossNRTTM
FROM dbo.tblMainTTMCalculations
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey = '202607';

-- ====== 4. #29 BHG — September fiscal year customer exist karta hai? ======
SELECT DISTINCT TOP 5 strCustomerName
FROM tblMain
WHERE strCustomerName LIKE '%Bankers%' OR strCustomerName LIKE '%Healthcare%';


READ-ONLY DIAGNOSTIC. No edits, no terminal. Read files, quote code with paths, findings only. Cover ALL four items below in ONE pass. No loop — read each relevant file once.

CONTEXT (already fixed & working): Interest Coverage TTM now = 1.90x (PBT TTM 350000, Interest TTM 387000, EBIT TTM 737000) after the TTM merge overwrite fix. Now resolve the remaining fields, all for customer AMERICAN CREDIT ACCEPTANCE (IndirectAuto), month 202607.

Legacy formulas (source of truth):
- perCashCollections = IIf(selection="Principal N/R", curCashCollections/curPrincipalNRPriorMonth, curCashCollections/curGrossNRorARPriorMonth)
- perNetChargeOffTTM = IIf(curAveragePrincipalNRTTM=0, 0, curNetChargeOffTTM/curAveragePrincipalNRTTM)
- perReserveCoverage = IIf(perNetChargeOffTTM=0, 0, perDiscountDividedByReserve/perNetChargeOffTTM)
- curNetChargeOffYTD = running SUM of curNetChargeOff within fiscal year

Investigate and quote ONLY:

=== ITEM 1: Cash Collections % (shows 0, legacy shows 6.56%) ===
- In frontend (monthSummaryRegistry.ts / mapIndirectAuto in monthSummaryProfiles / tblMainCalcs.ts): quote where "Cash Collections %" / perCashCollections is computed. What numerator/denominator? Does it read strPrincipalOrGrossCalculationSelectionCashCollection to pick Principal-vs-Gross? Quote the exact field keys (curPrincipalNRPriorMonth vs curGrossNRorARPriorMonth).
- Are curPrincipalNRPriorMonth / curGrossNRorARPriorMonth merged onto the row? Search where PriorMonth fields are set on series rows (backend read path / TryMerge*). If they're NOT merged (missing = 0), that's why the ratio is 0.

=== ITEM 2: TTM Net C/O % (panel shows 0.03%, top strip 2.89% — mismatch) ===
- Quote where "TTM Net C/O %" / perNetChargeOffTTM is computed (frontend). Which source does it read for curNetChargeOffTTM and curAveragePrincipalNRTTM — the merged tblMainTTMCalculations values, or tblMain columns (which may be 0/stale)?
- We just made TryMergeTtmIntoSeries always overwrite. Confirm curNetChargeOffTTM and curAveragePrincipalNRTTM are in that merge's dict (they are) and now reach the row. So why does the panel still show 0.03%? Is the panel computing from a different key, or is curAveragePrincipalNRTTM=0 causing the ratio to collapse? Quote the panel's exact computation.

=== ITEM 3: YTD Net C/O $ (shows 0) ===
- In SqlMainRepository RecomputeOtherYtdsAsync: quote it. Does it compute curNetChargeOffYTD as running SUM of curNetChargeOff within fiscal year? Is it CALLED in the save path (quote call site)? Does it write to tblMain.curNetChargeOffYTD?
- If it computes but frontend reads a different key for "YTD Net C/O $", note the key mismatch. Quote the frontend read for "YTD Net C/O $".

=== ITEM 4: #29 fiscal-year-end (BHG, Sept-30) ===
- Now that TTM is year-agnostic (trailing-12 crossing fiscal boundaries), does anything ELSE still filter by fiscal year that would break a Sept-30 customer? Search RecomputePbtTtmAsync (year-locked, tblMain) and RecomputeOtherYtdsAsync — do these still scope by intFiscalYear in a way that conflicts with the year-agnostic tblMainTTMCalculations? Quote the fiscal-year filters. State whether YTD (which SHOULD reset at fiscal year per legacy) vs TTM (year-agnostic) are handled correctly for a non-December fiscal year.

OUTPUT (for each item 1-4):
- The exact computation/formula in the new app (quoted, with file path).
- The broken link: missing merged field / key mismatch / wrong source / fiscal filter.
- The exact minimal fix location (file + what to change), but DO NOT write the fix.

End with a short table: Item | Root cause | Fix location. Then STOP. Findings only, no fixes.

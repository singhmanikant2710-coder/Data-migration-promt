John — Keystone Private Income Fund: every row is labelled one fiscal year later than the standard convention (e.g. April 2021 is stored as FY2022; standard would be FY2021). The other three exception customers use the starting-year pattern instead. Could you confirm how Keystone should be corrected before the cleanup?
Aur ye question add karo:
John — the save routine calls a query named qryMainYTDCaculations_008perInventoryTurn, which isn't in our copy of the database. Could you check whether it exists in the live Access database, and if so share its SQL? Until then we're leaving perInventoryTurn unchanged.

IMPLEMENTATION. Goal: make every calculation match the legacy Access
expressions I gave you, without changing anything else.

RULES (all batches):
- Change only the lines listed. No refactors, no signature changes,
  no renames, keep all alias lookups.
- Legacy expression is the spec. Zero denominator -> 0 (not NULL).
- After EACH batch: build backend + frontend. If build fails or you find
  anything not matching what I describe, STOP and report. Otherwise
  apply and continue to the next batch.
- Do not commit. Leave changes in the working tree.

BATCH 1 — SqlMainRepository.cs T-SQL persist (L4):
a. dblFixedChargeCoverage / TTM (4480-4497): fixed charges =
   CPLTD + InterestExpense only. Remove curDistributions(TTM) from the
   denominator. Keep it in the numerator (cash available).
b. perReserveCoverage (4694-4701): perDiscountDividedByReserve /
   perNetChargeOffTTM; perNetChargeOffTTM = 0 -> 0.
c. perIneligiblePercent (4721-4728): denominator and guard use
   curPrincipalNR, not curGrossNRorAR.
d. perNetIncomeYTDDividedByRevenueYTD (4731-4735): numerator
   curProfitBeforeTaxesYTD.
e. For fields whose legacy expression is IIf(x=0,0,...), change the
   zero branch THEN NULL -> THEN 0 (incl. perDebtDivTangibleNetWorth
   4466-4469). List every field you change.
f. curTotalAdjustedLiabilities (4434-4441): if curOtherB exists in the
   dev schema, remove the Related Party fallback. If not, don't touch;
   report.

BATCH 2 — SqlMainRepository.cs read backfill (L3):
a. Divide by the SIGNED value, keep the Math.Abs > 0 guard only:
   lines 1733, 1737, 1753, 1801, 1805, 1867.
b. dblAccountsReceivableTurnDays (1759-1760): remove Math.Round;
   use Math.Floor (VBA Int).

BATCH 3 — TblMainCalcs.cs (L2) + tblMainCalcs.ts (L1):
a. accessInt: Math.Floor / Math.floor. Fix the TS comment.
b. perNetIncomeYTDDividedByRevenueYTD numerator: curProfitBeforeTaxesYTD
   (cs:370, ts:343).
c. perNetChargeOffTTM in TS (280-281): banker's rounding via the
   existing intRound helper.
d. Keep the extra zero guards and extra "Principal N/R" string
   variants as they are.

BATCH 4 — StartupExtensions.cs:271: default ?? "Access" -> ?? "Sql".

BATCH 5 — TTM (P1). DIFF ONLY, DO NOT APPLY — I review first.
- Window: calendar trailing 12 months by strMonthKey order, customer-
  scoped, NO intFiscalYear filter (remove line ~3673 filter).
- Fewer than 12 months: sum available months; the two average fields
  average present rows (legacy Avg).
- Compute all TTM components first, then ratios that use them.
- On save of month X: recompute TTM and calculated columns for X
  through X+11.
- Write all 9 TTM components to tblMain AND keep writing
  tblMainTTMCalculations as today, so no existing read path breaks.
  Do not remove TryMergeTtmIntoSeries.

DO NOT TOUCH: perInventoryTurn (legacy formula unknown), SQL ROUND
half-away vs banker's in L4, intElapsedFiscalDays guard.

Final report: per batch, files + line ranges changed, build result,
and a SQL query I can run to compare stored values vs the legacy
expression for Batch 1 fields.

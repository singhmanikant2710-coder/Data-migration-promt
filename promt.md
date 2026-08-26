SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only the method RecomputeTtmCalculationsAsync. Show unified diff BEFORE applying. Do not run build.

CONTEXT: The method currently references curAverageGrossNR (in required[], in the CTE as AVG(curAverageGrossNR), and in MERGE). But curAverageGrossNR does NOT exist in tblMain, so required.Any(...) is true and the method returns early — nothing gets written. Client (John Halsrud) confirmed the correct monthly source is curGrossNRorAR. Fix: replace curAverageGrossNR with curGrossNRorAR everywhere it appears as a SOURCE column. Keep the OUTPUT column name curAverageGrossNRTTM unchanged (that's the target column in tblMainTTMCalculations).

EXACT CHANGES — replace the SOURCE column name only, in these two places:

1) In the required[] array:
   "curAverageGrossNR"   ->   "curGrossNRorAR"

2) In the CTE, the line:
   AVG(curAverageGrossNR) OVER (...) AS curAverageGrossNRTTM
   change ONLY the source inside AVG():
   AVG(curGrossNRorAR) OVER (...) AS curAverageGrossNRTTM
   (Keep the alias "curAverageGrossNRTTM" exactly — only curAverageGrossNR inside AVG() becomes curGrossNRorAR. Keep the same window clause.)

DO NOT change:
- The output alias curAverageGrossNRTTM (target column stays the same).
- The MERGE SET / INSERT / VALUES lines (they reference curAverageGrossNRTTM, the output — leave them).
- Any other column, the window clauses, COALESCE (none), or anything else.
- Only the two occurrences of the SOURCE name "curAverageGrossNR" -> "curGrossNRorAR".

VERIFY BEFORE SHOWING DIFF:
a) required[] now has curGrossNRorAR (not curAverageGrossNR), and all other required entries unchanged (11 total: strCustomerName, strMonthKey, curInterestExpense, curProfitBeforeTaxes, curDepreciation, curAmortization, curDistributions, curCPLTD, curNetChargeOff, curPrincipalNR, curGrossNRorAR).
b) CTE has AVG(curGrossNRorAR) ... AS curAverageGrossNRTTM.
c) MERGE SET/INSERT/VALUES unchanged (still reference curAverageGrossNRTTM output).
d) No other changes. Confirm curAverageGrossNR no longer appears anywhere in the method as a source.

Show the unified diff. Apply nothing until I confirm.

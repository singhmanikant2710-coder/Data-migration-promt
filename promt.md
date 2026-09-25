READ-ONLY — do not propose or apply any fix.

Covenant display bug: Summary Top Strip showed Min Tangible Net Worth as
"26207.22%" while Monthly Summary showed $26,207. Write path is already
fixed (tblMainCovenants.strCovenantActual). Now map the DISPLAY side.

Covenant keys: MinTNW, MinPBT, MinFCC, MaxDilution, MaxSeniorDebtTNW,
MinInterestCoverage, MaxCARatio.

For EACH key, one table row:
1. Backend: every place that chooses threshold vs actual for display
   (SqlMainRepository — ignore AccessMainRepository, it is dead code).
   Quote file:line and which one it prefers.
2. Frontend: every place that renders it (view, edit, report, Top Strip,
   Monthly Summary, covenant tile). Quote file:line, which value it
   picks (threshold/actual/computed), and the format ($, %, x).
3. MonthSummaryTable: the computed quotient (e.g. AdjLiab /
   MaxAdjDebtTNWLimit) that overrides stored values — which keys does
   it apply to? Quote it.
4. Legacy Access: which form/report shows each covenant, and does it
   display threshold, actual, or both? Quote the control source.

Output: one table (key × location), then a list of every place where
two screens show different values or formats for the same key.
Report only.

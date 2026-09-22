READ-ONLY — do not propose or apply any fix.

We're scoping the threshold-vs-actual display bug for all 7 keys in
thresholdKeys (MinTangibleNetWorth, MinProfitBeforeTaxes,
MinFixedChargeCoverage, MaxDilutionPercent, MaxSeniorDebtTNW,
MinInterestCoverage, MaxCARatio) — not just Min TNW.

For each of the 7 keys, list:
1. Every backend location that decides threshold-vs-actual preference for
   it (you previously found 4: two in SqlMainRepository, two in
   AccessMainRepository) — confirm the preference (threshold-first or
   actual-first) for each key in each location, and flag any
   inconsistency between the 4 copies for the same key.
2. Every frontend location (covNumeric in view/edit/report pages, plus
   MonthSummaryTable's computed-quotient path) and its preference for
   each key.
3. Which customers currently have non-null data for each of these 7
   covenant names, so we know the real-world blast radius per key
   (a query I can run, since you have no DB access).

Report as a table: key x location x preference. Do not recommend a fix
yet — I want the full map of what's currently inconsistent before we
decide anything.

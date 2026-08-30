READ-ONLY. Read once, quote, stop. No loop. Find what determines hasFiscalYear in GetMonthKeySeriesForYearAsync.

CONTEXT: GetMonthKeySeriesForYearAsync has two paths:
- hasFiscalYear=TRUE: WHERE intFiscalYear = @yr  (correct for BHG — 202601-202605 have intFiscalYear=2025)
- hasFiscalYear=FALSE: WHERE LEFT(strMonthKey,4) = @yrStr  (calendar year — WRONG for BHG, drops 202601+)

For BHG the dropdown only shows up to 202512, suggesting the FALSE (calendar) path runs. Need to confirm.

In the backend repository (GetMonthKeySeriesForYearAsync):
1) Quote the full method including how `hasFiscalYear` is computed/set. What makes it true vs false? Is it a column existence check, a config flag, a null check on intFiscalYear, or a parameter?
2) Quote how @yr and @yrStr are bound. Is @yr the fiscal year or calendar year?
3) Is there any condition where hasFiscalYear ends up FALSE even though intFiscalYear data exists (e.g. it defaults to false, or a try/catch, or a schema probe that fails)?

OUTPUT:
- A) The full method + how hasFiscalYear is determined, quoted.
- B) For BHG (which HAS intFiscalYear populated), would hasFiscalYear be TRUE or FALSE? Why?
- C) If it's FALSE despite intFiscalYear existing → that's the bug (calendar filter used, drops 202601+). If TRUE → the fiscal filter should already work, so the bug is elsewhere (quote @yr binding).
- D) Exact fix location.
- No fix. Findings only.

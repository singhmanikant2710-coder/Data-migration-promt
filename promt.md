READ-ONLY. No code changes.

Earlier we fixed YTD PBT: a stored YTD of exactly 0 was treated as
"missing" and replaced with TTM (TryMergeTtmIntoSeries). You reported
the same "zero treated as missing" pattern for YTD Revenue and YTD Gross
Profit in a routine that WRITES to the database.

Report:
1. Every place (backend + frontend) where curRevenueOrSalesYTD,
   curGrossProfitYTD (and aliases) are checked for missing/zero and
   replaced or recomputed. Quote file:line and the exact condition.
2. For each: does it only change the display, or does it WRITE to
   tblMain / tblMainYTDCalculations / any table? Which save path calls it
   and when?
3. What value gets written/shown when YTD = 0 (TTM? sum of monthly?
   something else)?
4. Legacy: how does Access compute YTD Revenue / YTD GP
   (qryMainYTDCalculations_*, funSave)? Does legacy ever replace a 0
   YTD? Quote it.
5. Impact: SQL query to find rows in tblMain where these YTD values are
   0 and the monthly values do not sum to 0 (possible damage already
   written), plus the count of rows where YTD = 0 genuinely.
6. Risk rating (low/medium/high) and the smallest safe fix, legacy
   parity. Do not apply.

Report only, with file:line and legacy quotes.

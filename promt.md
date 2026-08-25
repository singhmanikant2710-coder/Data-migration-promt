READ-ONLY DIAGNOSTIC. No edits, no terminal. Read files, quote code with paths, findings only.

CONFIRMED FACT (from SQL on the live DB): In tblMain, the monthly column curInterestExpense has correct values for all months (e.g. 202607=10000, 202606=10000, 202603=89000), BUT the precomputed aggregate column curInterestExpenseTTM is 0.00 for the newly entered months (202607, 202606) while older/migrated months have values (202605=351000, etc.). Same pattern for other TTM/YTD aggregate columns. So the aggregate columns are NOT being recomputed/persisted when a new month is saved in the new app. Legacy Access computes TTM on-the-fly from trailing-12 monthly values instead of relying on a stored column.

I need to determine the INTENDED design of the new app so I fix it in the right layer. Investigate and report ONLY:

1) DOES THE NEW APP RECOMPUTE TTM/YTD AT SAVE TIME?
   - In backend/src/Bcat.Infrastructure/**/*.cs (Access + SqlServer MainRepository) and Bcat.Application services, find the upsert/save path for a month row (UpsertRowAsync / batch save / MainController save). Quote it. Does it compute any TTM/YTD aggregate (curInterestExpenseTTM, curProfitBeforeTaxesTTM, curNetChargeOffTTM, curRevenueOrSalesYTD, etc.) before writing? Or does it only write the raw monthly fields the user entered?
   - Search backend for where "curInterestExpenseTTM" (and a couple other *TTM / *YTD column names) are ASSIGNED/written. Is there ANY code that populates them, or do they only ever come from migrated data? Quote every write site with path.

2) IS THERE A BACKEND TTM WINDOW-SUM ANYWHERE?
   - Search Bcat.Application (TblMainCalcs.cs and services) and Bcat.Infrastructure for any method that sums a monthly field across the trailing 12 months to produce a TTM (e.g. iterates rolling24 / GetRolling24Months and sums curInterestExpense). Quote it if it exists. If none exists backend-side, state that clearly.

3) HOW DOES THE FRONTEND GET INTEREST EXPENSE TTM?
   - In monthSummaryRegistry.ts / util.ts, for the "Interest Coverage TTM" (auto industry) and "EBIT TTM" / "Interest Expense TTM" display: quote the render/compute. Does it (a) read the precomputed curInterestExpenseTTM field directly, (b) compute via a rolling window like computeFccTtmFromRollingWindow, or (c) mix? 
   - Contrast with FCC TTM: FCC has computeFccTtmFromRollingWindow (sums monthly CAFC/Fixed/EBITDA/interest over 12 months). Does Interest Coverage TTM / EBIT TTM have an equivalent window-sum, or does it fall back to the precomputed *TTM field (which is 0 for new months)? Quote the exact fallback chain.

4) THE DECIDING QUESTION:
   - Based on 1-3: is the new app DESIGNED to (A) persist recomputed TTM columns at save time [and the bug is that save doesn't recompute them], or (B) compute TTM on-the-fly at read/render from monthly values [and the bug is that Interest Coverage/EBIT TTM lacks the window-sum that FCC TTM has]? 
   - Report which layer already does it correctly for at least one metric (FCC via frontend window-sum is the known-good example) so we can mirror that pattern.

OUTPUT:
- A) Save path: does it recompute TTM/YTD? yes/no + quoted code.
- B) Any backend window-sum for TTM? yes/no + quote or "none".
- C) Frontend Interest Coverage TTM / EBIT TTM source: precomputed field vs window-sum, with the exact fallback chain quoted.
- D) Verdict: is the correct fix layer (A) backend save-time recompute, or (B) frontend on-the-fly window-sum mirroring computeFccTtmFromRollingWindow? Recommend which matches the app's existing design, with evidence. 
- E) List which metrics need the same treatment (Interest Expense TTM, EBIT TTM, Interest Coverage TTM, Net C/O TTM, YTD Net C/O, Reserve Coverage, Cash Collections %).
- F) No fix yet. Findings only.

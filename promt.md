READ-ONLY. No edits. ONE pass. Trace the FULL flow of what happens when a month is selected and why BHG shows blank for 202606. Quote with file paths.

CONTEXT: BHG (Oct fiscal year). Fiscal year 2025 contains calendar months 202510-202606 (confirmed: 202601-202606 have intFiscalYear=2025). Month dropdown correctly lists up to 202606. Selecting 202601+ shows BLANK data. We changed GetCurrentYearSeriesAsync to filter by intFiscalYear=@yr, but it did NOT fix it. Need to find the REAL data path the UI uses for the selected month.

1) FRONTEND (frontend/src/app/blackbook/edit/page.tsx): when selectedMonthKey changes, what data does the page fetch/use? Quote every API call tied to the selected month/year. Specifically: does the edit page get its row data from GetCurrentYearSeriesAsync, or from a DIFFERENT endpoint (e.g. a single-month fetch, rolling-24, latestPoint, or /api/v1/metrics/... with monthKey)? List the exact endpoints called and what monthKey/year each receives.

2) Which of these produces "latestPoint" / "latestPointWithEdits" / the series the tiles render from? Trace from the API response to the object the tiles use. Quote where selectedMonthKey picks the row out of the series.

3) BACKEND: for EACH endpoint the edit page calls to get the selected month's data, quote its WHERE clause. Check ALL of these for calendar-vs-fiscal filtering:
   - GetCurrentYearSeriesAsync (already changed to intFiscalYear)
   - GetRolling24MonthsAsync
   - any single-month getter (GetMonthAsync / GetMetricPoint / by monthKey)
   - the controller endpoints under /api/v1/metrics/* that the edit page hits
   Quote each WHERE clause and whether it filters by LEFT(strMonthKey,4) (calendar), intFiscalYear (fiscal), or monthKey directly.

4) KEY: when selectedMonthKey=202606 and selectedYear=2025, which endpoint returns the row for 202606? Does the frontend request it with year=2025 (fiscal) but the backend still filters that specific call by calendar year 2025 (excluding 202606)? Find the ONE call whose result feeds the tiles and check ITS filter.

5) Also: how does the frontend pick the row for selectedMonthKey from the returned series? If the series is fetched by year and then .find(mk === selectedMonthKey), and the series still lacks 202606 (because THAT fetch is calendar-filtered), the tiles get nothing. Quote the .find/selection logic.

OUTPUT:
- A) Every endpoint the edit page calls for month data, with the monthKey/year each gets. Quoted.
- B) Which endpoint's result actually feeds the rendered tiles for the selected month. Quoted.
- C) That endpoint's backend WHERE clause — calendar or fiscal? Quoted.
- D) The ONE real reason 202606 is blank: which specific fetch excludes it (calendar filter, or a monthKey the series doesn't contain, or a .find that fails).
- One pass. Findings only. No fix.

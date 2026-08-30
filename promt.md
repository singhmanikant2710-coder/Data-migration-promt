READ-ONLY. No edits. ONE pass. Find EVERY place that filters tblMain by CALENDAR year (LEFT(strMonthKey,4)) instead of fiscal year (intFiscalYear). Quote each with file path.

ROOT CAUSE (confirmed): For BHG (October fiscal year), fiscal year 2025 = calendar months 202510-202609. But multiple parts of the app filter by CALENDAR year (LEFT(strMonthKey,4)='2025' = 202501-202512), which:
- Excludes 202601-202605 (fiscal 2025 but calendar 2026)
- Wrongly includes 202501-202509 (calendar 2025 but fiscal 2024)
Symptoms: (a) selecting 202601+ shows 202512's data; (b) Monthly Summary / Fiscal YTD shows 202501-202512 instead of the fiscal-year months.

Search BACKEND (backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs and any other repo/controller):
1) grep for LEFT(LTRIM(RTRIM(strMonthKey)),4) and LEFT(strMonthKey,4) — list EVERY method using it. For each, quote the method name + WHERE clause. Methods likely include: GetCurrentYearSeriesAsync (already changed?), GetPriorYearsSeriesAsync, GetMonthKeySeriesForYearAsync (fallback branch), and any YTD / summary / monthly-summary method.
2) For each, note whether it has an intFiscalYear alternative path or is calendar-only.
3) Find the method(s) behind the "Monthly Summary" / "Fiscal YTD" view specifically — what filters those rows? Quote its WHERE.
4) Find how maxMonthKey / latest month is computed (getMaxMonthKey or similar) — does it use MAX over calendar year or fiscal year, or overall max? Quote it.

Search FRONTEND (frontend/src/app/blackbook/edit/page.tsx):
5) How is the displayed row for the selected month chosen? Does it .find(monthKey===selectedMonthKey) in a series, or use maxMonthKey/last-of-list? Quote.
6) The Monthly Summary component — where does its month list come from? Quote the source (series/API) and any year filter.

OUTPUT — a checklist:
- A) EVERY backend method filtering by calendar year (LEFT(strMonthKey,4)), quoted, with whether each needs to become fiscal-aware.
- B) The Monthly Summary / Fiscal YTD data source + its filter, quoted.
- C) maxMonthKey computation — calendar or fiscal or overall-max, quoted.
- D) Frontend row-selection for selected month, quoted.
- E) A prioritized list of the exact changes needed to make everything fiscal-year consistent for non-December customers.
- One pass. Findings only. No fix.

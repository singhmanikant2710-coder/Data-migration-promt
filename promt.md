READ-ONLY. Read once, quote, stop. No loop. Find how the actual DATA (metrics/series) is fetched for a selected month/year, and whether it filters by calendar year.

REFINED SYMPTOM: The Month dropdown correctly shows all months up to 202606 (fiscal-year options work). BUT selecting 202601+ shows NO DATA in the UI, while 202512 and earlier show data. Legacy shows data for 202606. So the month-OPTIONS endpoint is fine; the DATA-FETCH endpoint fails for 202601+.

HYPOTHESIS: The data-fetch (series/metrics for the selected year) filters by CALENDAR year (LEFT(strMonthKey,4)=@yr), so for year=2025 it only returns calendar-2025 rows (202510-202512), and 202601-202606 (calendar 2026, fiscal 2025) return no data.

In backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs:
1) Find the method that fetches the metric series / rows for the edit screen by customer+year (likely GetCurrentYearSeriesAsync or similar — the one whose results populate the blackbook edit page tiles). Quote its WHERE clause.
2) Does it filter by LEFT(strMonthKey,4) = @yrStr (calendar) or by intFiscalYear = @yr (fiscal)? Quote exactly.
3) The frontend passes the selected fiscal year. If this data query uses calendar-year filtering, then for BHG year=2025 it returns only 202510-202512, and selecting 202601+ yields no row → blank UI. Confirm.
4) Also check GetRolling24MonthsAsync and any other data-fetch used by the edit page — do they filter by calendar or fiscal year? Quote.

OUTPUT:
- A) The data-fetch method's WHERE clause, quoted.
- B) Calendar-year or fiscal-year filter?
- C) Confirm: does year=2025 exclude 202601-202606 data for BHG (calendar filter)?
- D) Exact fix: change data-fetch filter to intFiscalYear = @yr (matching month-options and context/years, which are fiscal). List every method needing the same change.
- No fix. Findings only.

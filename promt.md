READ-ONLY. Read once, quote, stop. No loop. Find how the monthkey-series endpoint filters by year.

CONFIRMED via SQL: For BHG (October fiscal year), months 202601-202605 have intFiscalYear=2025 (same fiscal year as 202512). So fiscal year 2025 contains calendar months 202510-202609. But the Month dropdown for year=2025 only shows up to 202512 (calendar 2025), excluding 202601+ (calendar 2026 but fiscal 2025).

HYPOTHESIS: The backend monthkey-series filters by CALENDAR year (LEFT(strMonthKey,4)=year) instead of intFiscalYear, so 202601+ (calendar 2026) are dropped even though they belong to fiscal year 2025.

Find the backend endpoint for /api/v1/metrics/monthkey-series:
1) Locate the handler/repository method that returns month keys for a customer+year. Search "monthkey-series" or "MonthKeySeries" or "GetMonthKeys". Quote the SQL/filter it uses.
2) Does it filter by LEFT(strMonthKey,4) = @year (calendar year) OR by intFiscalYear = @year (fiscal year)? Quote the exact WHERE clause.
3) Compare: the frontend passes year (from the Year dropdown, which lists intFiscalYear values per /api/v1/context/years). So the frontend's "year" is a FISCAL year, but if the backend filters by CALENDAR year (strMonthKey prefix), there's a mismatch for non-December fiscal years.

OUTPUT:
- A) The monthkey-series filter WHERE clause, quoted.
- B) Does it filter by calendar year (strMonthKey prefix) or intFiscalYear? 
- C) Confirm the mismatch: frontend year = fiscal year, backend filter = calendar year → 202601+ excluded for BHG.
- D) Exact fix location: change the filter to use intFiscalYear = @year (matching how /api/v1/context/years defines the year).
- No fix. Findings only.

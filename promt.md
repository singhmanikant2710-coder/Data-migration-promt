READ-ONLY. Read once, quote, stop. No loop. Confirm the selectedYear derivation bug.

ROOT CAUSE FOUND: In frontend/src/app/blackbook/edit/page.tsx, when a monthKey is used to derive the year:
    if (/^\d{6}$/.test(mkp)) return mkp.slice(0, 4);
This takes the CALENDAR year (first 4 digits of YYYYMM). For BHG (Oct fiscal year), 202601 → "2026", but 202601 belongs to FISCAL year 2025. So selectedYear becomes 2026, the current-year API returns no rows for fiscalYear=2026, and the UI falls back to stale 202512 data.

1) Quote the FULL context around `return mkp.slice(0, 4);` — is this only for initial load (from URL param), or does it also run when the user changes the Month dropdown? Quote the surrounding function/effect.
2) When the user selects a month in the dropdown (onChange sets selectedMonthKey), does selectedYear get recomputed from the new monthKey? Trace: does changing selectedMonthKey trigger a selectedYear change via slice(0,4)? Quote the effect/handler.
3) Is the fiscal year available per row? Each MetricPoint has fiscalYear (we saw "fiscalYear": 2025 in the rolling24 response). So for a selected monthKey, we can get its fiscalYear from the series row (series.find(monthKey===sel).fiscalYear). Quote where series rows expose fiscalYear.
4) How does selectedYear drive the current-year fetch? Confirm the fetch uses selectedYear as the `year` query param.

OUTPUT:
- A) The slice(0,4) context — initial-only or also on month change? Quoted.
- B) Whether selecting 202601 sets selectedYear=2026 (calendar) instead of 2025 (fiscal). Confirmed?
- C) Is fiscalYear available on the series row for the selected month? Quoted.
- D) Exact fix: when deriving selectedYear from a monthKey, use the row's fiscalYear (from the series/rolling24 data) instead of slice(0,4). Name the exact location(s).
- No fix. Findings only.

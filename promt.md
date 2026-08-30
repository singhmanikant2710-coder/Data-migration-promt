READ-ONLY. Read once, quote, stop. No loop. Find how getMaxMonthKey / max-month-key is computed in the backend.

CONFIRMED FROM UI: Month dropdown shows 202510-202605 (correct). But "Month Key: 202512" is displayed and tiles show 202512's data, even though dropdown appears on 202605. "New Month: 202606" is shown. So maxMonthKey appears to be 202512 (calendar-year max), and the default-month effect (def = arr.includes(maxMonthKey) ? maxMonthKey : last) resets selectedMonthKey to 202512.

1) Find the backend endpoint/method for max month key (getMaxMonthKey → likely /api/v1/metrics/max-monthkey or similar → a repo method). Quote its SQL. Does it compute MAX(strMonthKey) overall, or MAX filtered by calendar year (LEFT(strMonthKey,4)), or something that returns 202512 for BHG?
2) BHG has months up to 202605 (all intFiscalYear 2025). Why would max return 202512? Is it: MAX over calendar year 2025 (LEFT=2025 → 202512), or MAX where some data column is non-null (202512 has PBT, 202601-202605 partially empty), or MAX of a "completed" month?
3) Quote how "New Month" (202606) is computed — this suggests the system knows the latest is 202605 (New Month = 202605+1). Compare with maxMonthKey logic.

Also FRONTEND:
4) Quote the default-month effect that sets selectedMonthKey from maxMonthKey. Does it run on every series/monthkey load and OVERWRITE a user's selection? Quote its dependency array and any guard.

OUTPUT:
- A) getMaxMonthKey backend SQL, quoted. Calendar-max or overall-max?
- B) Why it returns 202512 for BHG (calendar filter, or non-null data filter).
- C) The default-month effect that resets selectedMonthKey to maxMonthKey, quoted — does it overwrite user selection?
- D) Exact fix: (i) make maxMonthKey overall MAX(strMonthKey) or fiscal-aware, and/or (ii) stop the default effect from overwriting an explicit user month selection.
- No fix. Findings only.

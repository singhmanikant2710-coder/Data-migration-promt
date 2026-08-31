READ-ONLY. Read once, quote, stop. No loop. Find how "New Month" (value next to "Add New Month") is computed.

BUG CONFIRMED (legacy verified): In legacy, New Month is ALWAYS latest-existing-month + 1, constant regardless of the selected month (legacy shows Month 202603 selected but New Month 202606). In our app, New Month = selectedMonthKey + 1 (wrong): selecting 202603 shows New Month 202604 instead of staying at latest+1.

In frontend/src/app/blackbook/edit/page.tsx:
1) Find the "New Month" value near "Add New Month". Search "New Month" / newMonth / nextMonth. Quote how it's computed.
2) Is it based on selectedMonthKey + 1, or maxMonthKey + 1? Quote the expression and its dependency (does it recompute when selectedMonthKey changes?).
3) Quote how maxMonthKey (overall latest existing month for the customer) is available here — we need to base New Month on maxMonthKey, not selectedMonthKey.
4) Quote the YYYYMM increment logic — does adding 1 handle December→January rollover (202612 → 202701)?

OUTPUT:
- A) The New Month computation, quoted.
- B) selectedMonthKey+1 (bug) or maxMonthKey+1 (correct)? 
- C) The month-increment helper (YYYYMM +1) + does it handle Dec→Jan rollover, quoted.
- D) Exact fix: compute New Month from maxMonthKey + 1 (latest existing), independent of selectedMonthKey. Name the location.
- No fix. Findings only.

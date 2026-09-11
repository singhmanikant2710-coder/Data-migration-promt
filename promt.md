READ-ONLY. Two checks. Quote with paths.

CHECK 1 — FY switch month glitch: When switching Fiscal Year (e.g. 2026 → 2025), the previously-selected month (2026's) briefly persists before the correct latest month of the new year appears. Top Strip Month Key/Fiscal also briefly shows the old value.

1) In frontend/src/app/blackbook/edit/page.tsx, find the FY (year) dropdown onChange and how selectedMonthKey is reset when selectedYear changes. Quote it. When selectedYear changes, is selectedMonthKey cleared/reset immediately, or does it wait for the new series/monthkey-series to load (causing the stale flash)?
2) Is there an effect that, on selectedYear change, sets selectedMonthKey to the new year's latest month? Quote it and its timing. Does the old selectedMonthKey render until the new one is set?
3) Exact fix location: reset selectedMonthKey (or show a loading state) immediately on FY change so the stale month doesn't flash.

CHECK 2 — 60+ DPD % basis in Top Strip: Legacy shows 60+ DPD % on a PRINCIPAL N/R basis in the Top Strip for ALL customers.
4) In frontend/src/blackbook/expr/tblMainCalcs.ts, quote per60DPD — what denominator does it use (Principal N/R or Gross N/R)? Is it fixed to Principal, or selection-based?
5) In the Top Strip 60+ DPD % render (monthSummaryRegistry.ts), quote how it computes/picks the value. Does it force Principal N/R basis, or use a selection dropdown (which could give Gross for some customers)?
6) Confirm: for the Top Strip, is 60+ DPD % ALWAYS Principal N/R (legacy parity), or can it be Gross for some customers based on a selection field?

OUTPUT:
- CHECK 1: A) FY-change selectedMonthKey reset timing, quoted. B) Does old month flash before new? C) Fix location.
- CHECK 2: D) per60DPD denominator (Principal/Gross/selection), quoted. E) Top Strip 60+ DPD % render basis, quoted. F) Is it always Principal (legacy) or can be Gross?
- No fix. Findings only.

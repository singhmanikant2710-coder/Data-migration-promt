Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break. PDF conventions: @react-pdf, one .tsx per report; change this report
only, never shared defaults (pageSetup.ts, HtmlRichText.tsx).

TASK (UAT #217, Geoff) — CRM PD Grade Migration PDF formatting only:
1. "PD Grade Migration by Number of Accounts" (Matrix by Count): the BANK PD
   column values (row labels 3, 4, 5 … 14) font size 10 → 9.
2. "PD Grade Migration by Commitment ($MM)" (Matrix by Commitment): the
   "CAS PD Totals" row font size 10 → 9 (whole totals row incl. Bank PD
   Totals / # Changes / % Change cells).
3. Matrix by Commitment row height must match the Matrix by Count row height
   (same height for data rows; totals row consistent with the Count
   matrix totals row).
No data, calculation, colour, column width or header change.

PHASE 1 (read-only): file:line of both matrices in the PD Grade Migration
PDF component, current font sizes, row heights/padding of both matrices,
and any styles shared between them or with other reports.
PHASE 2: smallest change scoped to this report; if a style is shared by both
matrices, split only what's needed.
PHASE 3: implement.
PHASE 4: build frontend; render with real data and report page count before
vs after (taller Commitment rows may push content to another page).

Report:
1. What you found (file:line)
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. PDF before vs after (fonts, row heights, page count)
5. Edge cases: empty matrix / no data, many PD rows, long numbers
   (e.g. 2,518.7), landscape page fit
6. Other reports confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.

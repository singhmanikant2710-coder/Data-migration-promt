SINGLE-FILE, MINIMAL, BOUNDED EDIT. Only edit frontend/src/blackbook/pdf/BlackBookPdf.tsx. Do not touch other files. Do not refactor. Show unified diff BEFORE applying. Do not run build.

GOAL: Defect #26 — show ALL current-year and prior-year months (up to 12 each) on the PDF, matching legacy. Client wants them on the page like legacy (two stacked sections). Currently only ~6 render, and there's a slicing bug dropping the first 6 current-year months.

EXACT CHANGES:

1) FIX the monthlyChunks bug + make it a single full chunk. Current line:
   const monthlyChunks = seriesYearOnly.length > 0 ? [seriesYearOnly.slice(monthlyRowsOnFirstPage)] : [];
   Change to (render ALL current-year rows in one chunk, no truncation, no skip):
   const monthlyChunks = seriesYearOnly.length > 0 ? [seriesYearOnly] : [];
   (Note: the current code uses slice(monthlyRowsOnFirstPage) which SKIPS the first 6 rows — that's a bug. Using the full array fixes it and shows all months.)

2) Raise the two history caps from 6 to 12:
   - historyYearOnly.slice(0, 6)   ->  historyYearOnly.slice(0, 12)
   - historyYear2Only.slice(0, 6)  ->  historyYear2Only.slice(0, 12)
   (Find both .slice(0, 6) occurrences on the history grids and change to 12.)

DO NOT:
- Do NOT touch the disabled blocks (if (false && ...)).
- Do NOT change MONTHLY_FIRST_ROWS / MONTHLY_CONT_ROWS constants (they're now unused by the single-chunk change, leave them).
- Do NOT change page size, styles, panels, grid3, or JSX structure.
- Do NOT remove wrap={false} (keeps sections on one page).

VERIFY BEFORE SHOWING DIFF (report; don't force):
a) Confirm monthlyChunks is now [seriesYearOnly] and page 1 renders monthlyChunks[0] (all current-year rows). Confirm no continuation-page loop will fire (monthlyChunks.length stays 1).
b) Confirm both history .slice(0,6) became .slice(0,12), and nothing else changed.
c) Report: with wrap={false} on these tables, if the combined content (12 + 12 rows + panels) exceeds one page height, what happens in @react-pdf — does it clip, or push to a second page? Just report the risk; do not change layout yet.

Show the unified diff. Apply nothing until I confirm.

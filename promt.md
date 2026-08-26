SINGLE-FILE, MINIMAL, BOUNDED EDIT. Only edit frontend/src/blackbook/pdf/BlackBookPdf.tsx. Show unified diff BEFORE applying. Do not run build. Do not touch other code.

CONTEXT: The rolling-24 PDF = current fiscal year (12) + prior fiscal year (12) = 24 months, in TWO sections (matching legacy). Our earlier change also raised the FY-2 ("Two Fiscal Years Back" / "Historical Summary - {prevYear2}") section from 6 to 12, which added an extra full 2024 block that overflows to a second page. FY-2 is NOT part of the 24-month rolling view.

EXACT CHANGE — revert ONLY the FY-2 cap back to 6 (keep current-year full and FY-1 at 12):

Find:
  {historyYear2Only.slice(0, 12).map((r: MetricPoint, idx: number) => (
Change to:
  {historyYear2Only.slice(0, 6).map((r: MetricPoint, idx: number) => (

DO NOT change:
- monthlyChunks (keep [seriesYearOnly] — current-year full)
- historyYearOnly.slice(0, 12) (keep FY-1 at 12)
- Any headings, panels, styles, or the FY-2 conditional wrapper itself
- Only the FY-2 .slice(0, 12) -> .slice(0, 6)

VERIFY BEFORE SHOWING DIFF:
a) Confirm this is the ONLY change (FY-2 slice 12->6).
b) Confirm current-year is still [seriesYearOnly] (full) and FY-1 is still slice(0,12).

Show the unified diff. Apply nothing until I confirm.

READ-ONLY. No edits. Read frontend/src/blackbook/pdf/BlackBookPdf.tsx ONCE. Quote current state, stop.

CONTEXT: After raising history caps to 12, the PDF now spills to 2 pages and shows a "Two Fiscal Years Back" (FY-2) section that adds a full 12-month 2024 block. Legacy "rolling 24 months" = current fiscal year (12) + prior fiscal year (12) = 24 total, in TWO sections. The FY-2 (two years back) third section appears to be extra and is causing overflow to page 2.

Quote verbatim:

1) The three history-related arrays and their current slices: seriesYearOnly (current), historyYearOnly (FY-1), historyYear2Only (FY-2). Show each definition and where each is rendered (the JSX section with its heading text).

2) The section headings currently rendered: "Monthly Summary (Current...)", "Historical Summary — <year>" / "Prior Fiscal Year", and the "Two Fiscal Years Back" one. Quote each heading and the condition (e.g. {historySecond.length > 0 && ...}) that decides whether that section renders.

3) Is the FY-2 / "Two Fiscal Years Back" section wrapped in its own conditional block that could be removed or gated to keep only current + prior (24 months across 2 sections)? Quote that block's opening/closing.

4) How many total data rows are on page 1 currently vs what pushes to page 2 — based on the render order, which section is the first to overflow (the FY-2 block, or the panels)? Quote the render order of sections top to bottom.

OUTPUT:
- A) The 3 arrays + their slices + their headings, quoted.
- B) The exact conditional block for the FY-2 ("Two Fiscal Years Back") section, quoted — so I can see how to remove/gate it.
- C) State plainly: to match legacy "24 months = current + prior only" on fewer pages, should we (i) drop/gate the FY-2 section, and/or (ii) keep FY-2 but on its own page? Which sections in what order currently cause the 2-page spill?
- No fix yet. Findings only.

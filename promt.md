Two fixes on the Blackbook report:

1. REGRESSION from the last change: Rolling 24 mode now also shows 18
   months. Apply the fiscal-year row selection (fiscalYtdRows /
   fiscalPriorRows) ONLY when Fiscal YTD is selected. In Rolling 24 mode
   use the previous rolling24 behaviour exactly (24 months). Pass the
   selected mode into BlackBookPdf explicitly; do not infer it from data.

2. Back navigation: after opening the preview (View/Print Preview) and
   clicking Back, the report options reset to defaults (Fiscal YTD +
   Summary). Preserve the user's selections (Fiscal YTD / Rolling 24,
   Summary / Detail, and the other option groups) — keep them in the URL
   query params so Back restores them. Defaults apply only on first
   open with no params.

Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is

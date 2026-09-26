Fiscal YTD report (report page, Summary PDF) shows 24 months for ATHENS
202603 (202404-202603). Legacy shows 18: current FY to date
(202510-202603) + the full prior FY only (202410-202509).

1. READ-ONLY first: quote how the Fiscal YTD history rows are selected
   (prior-years endpoint, beforeYear param, any slice/filter).
2. FIX: in Fiscal YTD mode, show only rows with intFiscalYear =
   selected fiscal year (up to the selected month) and
   intFiscalYear = selected fiscal year - 1. Do not change Rolling 24.
Generic for all customers. Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is

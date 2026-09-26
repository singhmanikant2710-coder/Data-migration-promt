FIX the intermittent Blackbook PDF (report/page.tsx). Minimal, safe
changes only:

1. YTD Sales / YTD PBT columns: use the row's stored
   curRevenueOrSalesYTD / curProfitBeforeTaxesYTD first (legacy reads
   the stored value). Use sumYtdForRow only when the stored value is
   null. This removes the dependency on the captured series.
2. Build columns / columnsHistory with useMemo from the SAME arrays
   passed to BlackBookPdf (enrichedSeries, history, rolling24), instead
   of effect + setState, so closures can never be stale.
3. One readiness flag: Download button and autoDownload must wait until
   current-year, rolling24 (if selected), prior-years AND the covenant
   merge have all finished (success or error). autoDownload fires only
   once (useRef guard).
4. Pass noCache: true on the 3 metrics fetches, same as the edit page.
5. report/page.tsx:230 selectedYear uses the calendar year. Use the
   fiscal year for the selected month (customer fiscal start). If the
   fiscal start is not available on this page, report and do not apply.
Build, tests, do not commit.

REPORT FORMAT (mandatory):
- ADDED: new logic/lines (file:line)
- REMOVED: logic/lines removed (file:line)
- BEHAVIOUR CHANGE: what the user sees on screen, incl. NULL value case
- NOT TOUCHED: related code deliberately left as-is

TASK:
ATHENS PAPER COMPANY INC — Add New Month 202604. Before any save, the UI
(Summary Top Strip and Monthly Summary) shows Min Tangible Net Worth
62,297 and Min Net Income 8,369 — these are 202603 values. Also after F5.
Legacy shows blank for a new month.

Verified facts (do not re-investigate):
- DB is correct right after Add New Month: tblMainCovenants 202604
  actuals = NULL (Min TNW, Min Net Income, Other 1 (%)); tblMain 202604
  slots = Min TNW / Min Net Income with dblCovenantActual1/2 = NULL;
  strCustomField1/2 = NULL.
- API responses (summary, current-year, rolling24) contain NO value for
  these columns for 202604.
=> The FRONTEND fills 202604 from another month.

Check these first and quote the exact line that produces 62,297:
- latestPoint / latestPointComputed selection (edit/page.tsx ~1227-1233)
  falling back to the latest month with data instead of the selected month
- Top Strip covenant tile slot fallback reading latestPointComputed
  (edit/page.tsx ~1560-1610)
- any pick / latestValueUpToRow / carry-forward that searches earlier
  rows when the selected month has no value
- monthlyTopStrip memo missing latestPointComputed in deps (~1469-1487)

FIX: show the SELECTED month's value only; missing -> "—". Add the
missing memo dependency. Generic for all customers and industries.

RULES (mandatory):
1. READ-ONLY FIRST: show root cause (file:line) before changing code. If
   not proven, STOP and report.
2. SMALLEST FIX: only the lines causing this. No refactor or cleanup.
3. Legacy is the spec: never show another month's value; never fabricate 0.
4. DO NOT TOUCH: backend, save calculations, stored data, APIs, caches,
   other templates.
5. GOLDEN REGRESSION (before AND after, show table):
   - ATHENS 202603: Min TNW 62,297, Min Net Income 8,369, FCC TTM 4.98,
     A/R Turn Days 61, Inventory Turn 57
   - ECLIPSE 202604: Min TNW $401,175, Max Senior Debt/TNW 3.44x,
     Min Interest Coverage 1.84x, Max c/o (%) 0.03%
   - WESTLAKE 202604: Other 1 (%) 9.06, Max CAR Ratio 11.28%
   - MIDDLE GEORGIA 202011: Net C/O TTM 7.03%, Loan Loss Reserves % 9.59%
   - MAMMOTH MEDICAL: unchanged
   - ATHENS new 202604 + one other industry new month: covenants and
     custom fields "—" before save
   If ANY golden value changes, STOP — do not apply.
6. Build + tests. Do not commit. Revert next-env.d.ts / tsbuildinfo.

REPORT:
- Root cause (file:line)
- ADDED / REMOVED (file:line)
- BEHAVIOUR CHANGE per screen, incl. NULL case
- Golden regression table: item | before | after
- NOT TOUCHED

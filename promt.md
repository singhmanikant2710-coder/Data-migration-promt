TASK — Add New Month must start empty, exactly like legacy (all
customers, all industries).

Legacy (verified on 2-3 customers): after Add New Month every input
column shows 0 / blank; nothing comes from the previous month. As the
user types, same-row fields update live; YTD/TTM/prior-month fields
update only after Save. Legacy INSERT copies only 10 identity/fiscal
columns from tblCustomer (Form_frm001TruckingMain.bas:318).

Today some values on a new month are wrongly calculated (not all 0 /
blank) before any input.

STEP 1 — READ-ONLY: Add New Month on ATHENS 202604 + one other industry
customer. List every tile/column that is NOT 0 / "—" before typing,
with its source (file:line) — e.g. values seeded by the add-month
template, carried from another month, TTM/YTD estimates, covenant or
custom-field carry-over.

STEP 2 — FIX (generic):
- New tblMain row: identity/fiscal columns as legacy; financial input
  columns = 0 (as today's template); custom fields and covenant actuals
  = NULL. Nothing copied from any other month.
- Before save: inputs show 0; class A fields computed live from the
  entered values (0 -> 0); class B fields (YTD, TTM, prior month,
  averages, A/R Turn Days, Inventory Turn, GPM YTD, etc.) show "—" until
  the first Save, then the backend values.
- Covenants and custom fields show "—" until entered.

RULES:
1. Read-only first; show root cause before changing code.
2. Smallest fix; no refactor.
3. Do not change backend save calculations, existing months' data, or
   anything outside the add-month / new-month display path.
4. GOLDEN REGRESSION (before AND after table):
   - ATHENS 202603: Min TNW 62,297, Min Net Income 8,369, FCC TTM 4.98,
     A/R Turn Days 61, Inventory Turn 57
   - ECLIPSE 202604: Min TNW $401,175, Max Senior Debt/TNW 3.44x
   - WESTLAKE 202604: Other 1 (%) 9.06
   - MIDDLE GEORGIA 202011: Net C/O TTM 7.03%
   - ATHENS new 202604: all 0 / "—" before input; after entering legacy
     values and Save, values match legacy 202604
   If any existing-month golden value changes, STOP.
5. Build + tests. Do not commit. Revert next-env.d.ts / tsbuildinfo.

REPORT: root cause (file:line), ADDED / REMOVED (file:line), BEHAVIOUR
CHANGE (new month before input / while typing / after save, incl.
NULL), golden table, NOT TOUCHED.

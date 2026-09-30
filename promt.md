STRICT-SCOPE FIX — Customer view page, covenant "Order" column.

Issue (John): the covenant Order value is visible in the dropdown (edit)
but the Order column in the covenant grid/list on the Customer view page
is blank. Order values are 0..9; 0 is a valid value and must show "0".

STEP 1 — READ-ONLY: trace Order from API response -> model -> grid
column -> render. Quote file:line where it is lost (wrong property name,
`value || ""`, falsy 0, missing column binding, hidden column, etc.).
Confirm in the API response which field carries it.

STEP 2 — FIX: only make the Order value display in that column, exactly
as stored (0 shows "0", NULL shows blank).

RULES (mandatory):
- Change ONLY the display of Order on the Customer view page.
- Do NOT touch: the dropdown, save/update, sorting, covenant name ->
  format behaviour, other columns, backend, APIs, data, caches, any
  other page.
- Smallest diff. If the fix needs anything outside this, STOP and report.

TEST: 3 customers (ATHENS PAPER, WESTLAKE SERVICES LLC, MIDDLE GEORGIA
MANAGEMENT SERVICES INC) — Order column equals tblCustomer covenant order
values, incl. 0; dropdown and save work as before.
Build, tests, do not commit.
REPORT: root cause (file:line), ADDED/REMOVED (file:line), BEHAVIOUR
CHANGE incl. 0 and NULL, NOT TOUCHED.

SELECT strCustomerName,
  strCovenantAName, strCovenantAOrder, strCovenantBName, strCovenantBOrder,
  strCovenantCName, strCovenantCOrder, strCovenantDName, strCovenantDOrder
FROM tblCustomer
WHERE strCustomerName IN ('ATHENS PAPER COMPANY INC','WESTLAKE SERVICES LLC',
                          'MIDDLE GEORGIA MANAGEMENT SERVICES INC');

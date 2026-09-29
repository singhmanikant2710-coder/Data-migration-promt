FIX — stale React state after Add New Month (frontend only).

Evidence: after Add New Month 202604 (ATHENS) the page shows 202603's
covenant actuals and custom fields. No API response contains those
values (checked in Network with cache disabled). After Ctrl+R they are
gone. So page state from the previous month survives the add-month flow.

FIX (smallest, generic): after POST /api/v1/main/month succeeds and
caches are invalidated, do a FULL page reload of the edit page for the
new month (window.location.assign / replace with the same URL params +
monthKey = new month + the existing unsaved marker). Do not try to reset
individual state pieces. Legacy parity: legacy requeries the form after
adding a month.
Keep the unsaved-month marker working after the reload (class B "—"
until first Save).

RULES: frontend only; no change to backend, calculations or data;
smallest diff.
GOLDEN: ATHENS 202603 unchanged (62,297 / 8,369); ATHENS Add New Month
202604 -> immediately all 0 / "—" (covenants + custom fields) without a
manual reload; one other industry customer same; enter values -> class A
live, Save -> legacy values.
Build, tests, do not commit.
REPORT: ADDED/REMOVED (file:line), BEHAVIOUR CHANGE incl. NULL, NOT
TOUCHED.

SELECT curGrossProfit, curGrossProfitYTD
FROM tblMain WHERE strCustomerName LIKE 'ATHENS PAPER%' AND strMonthKey = '202604';

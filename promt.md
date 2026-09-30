REGRESSION: changing a covenant's Order on the Customer page no longer
changes the covenant order in the Blackbook (it worked before). Cause
(verify): Blackbook now reads tblMain slots (strCovenantName{i} /
dblCovenantActual{i}) and those slots are only (re)written for a NEW
month (SeedFromLatestAsync returns early when covenant rows exist). An
order change updates tblMainCovenants.intCovenantOrder but never
re-mirrors tblMain slots.

Legacy (funSave, Form_frm001TruckingMain.bas:715-739): on every save it
clears the slots ("0101 Historical Covenant Clear Update") and refills
slot i from the covenant with intCovenantOrder = i.

STEP 1 — READ-ONLY: quote which endpoint the Customer page order change
calls, which rows it updates (tblCustomer template? tblMainCovenants for
which month(s)?), and where tblMain slots are written today.

STEP 2 — FIX: after an order change (and after any covenant save), for
every month whose tblMainCovenants rows were changed, re-mirror that
month's tblMain slots: clear the slot columns, then write slot i from
that SAME month's covenant with intCovenantOrder = i (1..N), using that
month's own actuals. Reuse the existing clear + order-keyed write code
(extract it into one method), do not duplicate logic. Invalidate caches
as today.

RULES: no change to new-month seeding behaviour, values, calculations;
existing months' actuals must stay as saved.
GOLDEN: ATHENS 202603 swap Min TNW (1) and Min Net Income (2) -> Blackbook
shows Min Net Income first, values 8,369 / 62,297 unchanged; swap back ->
original. ECLIPSE and WESTLAKE unchanged. ATHENS Add New Month still
blank covenants.
Build, tests, do not commit.
REPORT: root cause (file:line), ADDED/REMOVED (file:line), BEHAVIOUR
CHANGE incl. NULL, NOT TOUCHED.


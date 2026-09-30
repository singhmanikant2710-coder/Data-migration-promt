STRICT-SCOPE FIX — Customer page, covenant "Reported" column.

Same symptom as the Order column you just fixed: the Reported value
(tblMainCovenants / tblCustomer strCovenant{X}Reported, e.g. "Monthly",
"Quarterly") shows when the dropdown is opened, but the closed control
looks blank/clipped. All customers.

STEP 1 — READ-ONLY: quote the Reported select (CustomerEditParts.tsx
file:line), its className, the column/td widths, and the CSS cascade
from the built stylesheet (which class wins for width and padding), and
compute the content box like you did for Order. Also confirm the value
is present in the DOM (data path fine) — if the data path is the cause,
quote that instead.

STEP 2 — FIX: smallest change so the stored Reported value is fully
visible in the closed control (e.g. remove the offending padding class
or give the column enough width). Do not truncate the text.

RULES:
- Only the Reported column display on the Customer page.
- Do NOT touch: the dropdown options, onChange/save, Order column,
  other columns' widths, sorting, backend, APIs, data, other pages.
- If the fix needs more than that, STOP and report.

TEST: ATHENS PAPER, WESTLAKE SERVICES LLC, MIDDLE GEORGIA MANAGEMENT
SERVICES INC — Reported shows "Monthly"/"Quarterly" as stored; Order
column still shows its number; save works.
Build, tsc, do not commit.
REPORT: root cause (file:line + CSS evidence), ADDED/REMOVED
(file:line), BEHAVIOUR CHANGE incl. NULL/blank value, NOT TOUCHED.



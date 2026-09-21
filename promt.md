Context: CASRR project, Distribution Parties maintenance screen (already
updated with Employee ID column instead of Role).

Small UI tweak: reorder the table columns so EMPLOYEE ID appears before NAME.
Current order: EMAIL (KEY) | NAME | EMPLOYEE ID | ACTIONS
New order: EMAIL (KEY) | EMPLOYEE ID | NAME | ACTIONS

Apply the same order change to the Add/Edit form's field layout if it
currently shows Name before Employee ID, for consistency.

Only touch column/field ordering — no logic changes.

Read-only. No edits. No plan. Just report with file paths + code.

Feature: Add a new "Distribution Parties" page to the left-hand MAINTENANCE menu, backed by dbo.[03_LIBRARY_10_Distribution Parties]. It must follow the SAME pattern as the existing maintenance pages (e.g. Loan Codes, NAICS, CAS Users, Selections).

Report using an existing maintenance page (pick Loan Codes or CAS Users) as the reference:
1) Frontend: the page component file path and route for that maintenance item, and where it's registered in the left MAINTENANCE menu (the nav config file). Show the menu entry.
2) Frontend: the list UI pattern used (table, stats bar, pagination, skeleton, toasts) and the API service call it makes.
3) Backend: the controller + repository that serves that maintenance table's list/CRUD. Show the endpoints and the SQL.
4) List exactly which files must be created/edited to add a Distribution Parties maintenance page (read-only list is enough for this UAT — client says "for future editing", so a list view registered in the menu satisfies it; full CRUD optional). Confirm whether read-only list or full CRUD is the smaller correct scope.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.

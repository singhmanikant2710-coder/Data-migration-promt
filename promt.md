Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #224, Geoff): Maintenance → Loan Codes screen. The "Code" column
values (e.g. AEP, ABE, ALC) render visibly smaller than the "Category"
column values ("Acct Type"). Make the Code column font size (and weight,
if different) match the Category column exactly. Text/styling only.

Rules:
- Change the Loan Codes screen only. If the Code cell uses a shared
  component/class (badge, monospace chip, shared DataTable column style)
  that other Maintenance screens also use, do NOT change the shared
  style — scope the override to Loan Codes and tell me which other screens
  use it.
- No change to data, search, filter, sorting, pagination, Add/Edit/Delete,
  modals or column widths (unless the larger text would wrap — then report).

PHASE 1 (read-only): file:line of the Loan Codes page, the Category and Code
cell rendering, their classes/styles (font size, weight, family), and any
shared component involved + its other users.
PHASE 2: smallest change, reuse the exact Category cell classes.
PHASE 3: implement.
PHASE 4: build frontend.

Report:
1. What you found (file:line)
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen before vs after (Code vs Category text size)
5. Edge cases: long codes, empty code, Add/Edit Loan Code modal unaffected,
   narrow screen
6. Other Maintenance screens confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged; no harness files left)
Do not commit or push.

Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #200 + #201, Geoff):
- Review Queue table: add a "Customer #" column between "Review Id" and
  "Borrower Name / Linesheet".
- Review Progress table: add the same "Customer #" column in the same position.
- Value = Customer_number from [02_CORE_02_Reviews] for that review.
- Display the value exactly as stored (text; keep leading zeros, no number
  formatting). NULL/empty → blank cell.
- Column header text exactly: "Customer #".
- Do NOT add it to Review History or any other screen (not requested).

Expected behaviour of the new column on both screens:
- Sortable like the neighbouring columns (string sort, NULLs last or same
  rule the grid already uses).
- Included in the "Filter rows" text search, so typing a customer number
  finds the row.
- Works with the UAT #178 persisted list state: the new sort key must be
  accepted by the sessionStorage sanitizers in lib/reviewReturn.ts
  (sanitizeReviewQueueListState / sanitizeReviewProgressListState), and an
  older stored state without it must still restore fine.
- Any export (CSV/Excel) of these grids, if one exists: tell me in Phase 1
  and do NOT change it unless I confirm.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
- Trace end to end with file:line: Review Queue page + Review Progress page
  (column definitions, sort, filter-rows logic) → API endpoints → service →
  repository SQL → DTO/contract.
- Confirm the exact column name/type of Customer_number in 02_CORE_02_Reviews
  and whether it is already selected/returned anywhere in these queries.
- Identify every other caller of the same endpoints/DTOs/SQL/DataTable
  config, and confirm they're safe with an added field.
- Report any existing bugs/risks; don't fix outside this task.
- If ambiguous (e.g. Customer_number missing on some rows, different column
  name, the "All" bucket duplicates), STOP and ask with A/B/C options.

PHASE 2: PLAN
- Smallest change: add CustomerNumber to the existing query/DTO/type and one
  column definition per screen. Reuse existing column/sort/filter patterns.
- No schema change. No new endpoint.

PHASE 3: IMPLEMENT
- Touch only what this task needs. Match each file's existing style.
- All other columns, widths, order, sorting, filters, tiles, My View,
  sample dropdown and pagination unchanged.

PHASE 4: VERIFY + REPORT
- Build backend and frontend (report file-lock errors separately).
- Verify with real functions/data where possible.

Report:
1. Root cause / what you found (file:line), plus any other issues noticed
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen behaviour: before vs after (both screens)
5. NULL / empty / edge cases: Customer_number NULL, empty string, leading
   zeros, very long value, same customer on two reviews, sort with NULLs,
   filter-rows by partial customer number, restored #178 state with and
   without the new sort key
6. Other screens/reports checked and confirmed unaffected (incl. Review
   History, Review Form, exports)
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.

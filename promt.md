Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #209 + #210, Geoff) — Review Form UI text only.

#209 Transactions section labels (bank is discontinuing "TTBA"). You already
found them in TransactionsSection.tsx (~:903, :926, :938, :942, :954, :958).
Change the visible labels AND matching aria-labels exactly:
a. "Total To Be Approved (TTBA)"   → "Household Exposure"
b. "TTBA Approver"                 → "RCA Approver"
c. "TTBA Approval Authority Level" → "RCA Approval Authority"
d. "TTBA Approval Reason"          → "Approval Reason"
(If the UI uppercases labels via CSS, keep that styling.)

#210 CRM Findings section: the header button "Add Row" must read
"+ Add Finding" and use the same font size / button style as the other
"+ Add ..." buttons on the Review Form (find them, e.g. in Covenants /
Policy Exceptions / Transactions, and reuse their exact className or
component). Behaviour of the button (what it adds, permissions, disabled
state in read mode) must stay exactly the same.

Rules:
- Text/styling only. No change to data, DB columns, API fields, property
  names, variables, validation or save logic.
- Do not change the CAS Linesheet / CRM Summary PDFs (separate items).
- Help-tip text stored in the DB that mentions TTBA: list it, don't change.

Work in 4 phases:
PHASE 1 (read-only): file:line for every label above, the "Add Row" button,
and the "+ Add ..." buttons you will mirror (with their classes). List any
other visible "TTBA" left on the Review Form (validation messages,
placeholders, tooltips, help tips) — do not change them. If a decision is
needed, STOP and ask with A/B/C options.
PHASE 2: smallest change; reuse existing button component/classes.
PHASE 3: implement.
PHASE 4: build frontend (backend only if touched).

Report:
1. What you found (file:line), plus remaining TTBA occurrences NOT changed
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen before vs after (Transactions labels in edit + read mode; CRM
   Findings button in edit + read mode)
5. Edge cases: empty values, read-only/locked review, narrow screen label
   wrapping, button disabled state
6. Other screens confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.

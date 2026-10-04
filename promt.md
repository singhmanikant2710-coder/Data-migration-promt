Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #211, Geoff): Link the Review Form "Checklist" section help tip
(ⓘ icon) to a new Help Tips library row. The row already exists in QA; it
does NOT exist in Dev. Dev [Help Tips] table currently has Help_tp_id 1-12
(form "02_SAMPLE LOAD_01_Main" / "04_REVIEW FORM_04", topics Customer Info,
Transactions, Covenants, Policy Exceptions, Regulatory Flags, Collateral,
Repayment, Scorecard, Risk Rating Justification, Key Risks, Unsatisfactory
Ratings) — no Checklist row.

New help tip (from Geoff):
- FORM:  04_REVIEW FORM_04
- TOPIC: Checklist Questions
- HELP TIP (title in bold, then text):
  **Checklist Questions**
  Answer each question either Yes, No, or N/A. Follow included guidance (if
  applicable) when answering the checklist question. "No" responses require
  supporting comments. If you initially assign a "No" response and revise
  the response to "Yes" or "N/A", remove any prior comments that were input.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
- Help Tips table: exact name, all columns + types, identity on Help_tp_id?,
  and how the tip text is stored in existing rows (plain text vs HTML, how
  bold/line breaks are written) — show one existing row's text format.
- How existing sections fetch their tip (by form+topic string? by id?) with
  file:line, e.g. TransactionsSection.tsx ~:884 / :375 (listLibrary()).
- Checklist section component: does it have an ⓘ icon today? What does it
  show/fetch now? file:line.
- Maintenance → Help Tips screen: will the new row show and be editable
  there?
- If a decision is needed, STOP and ask with A/B/C options.

PHASE 2: PLAN
- SQL: one idempotent script scripts/sql/insert-help-tip-checklist-questions.sql
  — INSERT only IF NOT EXISTS (same form + topic); never update/delete
  existing rows; no hard-coded Help_tp_id if identity; text formatted the
  same way as existing rows (bold title + paragraph). Safe to run on Dev, QA
  (already has it → no-op) and Prod.
- Frontend: wire the Checklist ⓘ icon exactly the way other sections do
  (same lookup by form + topic "Checklist Questions", same modal). Reuse
  existing helpers, no duplicate logic.
- No schema change. Do not run the script against any DB — I will run it.

PHASE 3: IMPLEMENT — touch only what this task needs.

PHASE 4: VERIFY + REPORT — build frontend (and backend if touched).

Report:
1. What you found (file:line), table/columns, text format
2. ADDED lines (file:line) incl. the full SQL script
3. REMOVED / CHANGED lines (file:line)
4. On-screen before vs after (Checklist ⓘ icon + modal)
5. Edge cases: tip row missing (Dev before script) → what the icon shows;
   script run twice; tip edited later in Maintenance → Help Tips shows new
   text; read-only/locked review
6. Other sections' help tips confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.

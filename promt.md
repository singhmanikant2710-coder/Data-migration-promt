Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #212, Geoff): On the Review Form, the "Distributed" and
"Finalized" date fields must be LOCKED (not editable) while the
"Mgr Approval" date field is empty (NULL).

Expected:
- Edit mode, Mgr Approval date empty → Distributed and Finalized date
  inputs are disabled (same disabled styling the form already uses) with a
  short hint/tooltip, e.g. "Enter Mgr Approval date first".
- As soon as the user enters a Mgr Approval date (even before Save), both
  fields unlock. If the user clears the Mgr Approval date again, they lock.
- Backend must enforce the same rule: a save that sets Distributed or
  Finalized date while Mgr Approval date is NULL is rejected with a clear
  validation message (UI-only locking is not enough).
- Existing data must never be cleared or changed automatically.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
- file:line for the three fields on the Review Form (which section/
  component, DB columns e.g. Review_approval_date / Review_distributed_date /
  Review_finalized_date), how they are edited and saved (UI → API →
  service → repository/SQL).
- Every OTHER path that sets Distributed/Finalized dates (Email / Initial
  Memo / Final Memo actions, status changes, bulk updates, Review Progress,
  admin tools, SQL). Report each with file:line.
- How Review Status / tiles (Approved, Distributed, Finalized) are derived
  from these dates, and confirm the rule doesn't change status logic.
- Count in the DB / describe how to find existing reviews where Distributed
  or Finalized is set but Mgr Approval is NULL (give me the SQL; don't run
  against Prod).
- STOP and ask me with A/B/C options + trade-offs on:
  1. Existing rows with Distributed/Finalized already set but Mgr Approval
     NULL: show those fields locked (values kept) vs editable so users can
     fix them vs something else.
  2. If Mgr Approval date is cleared while Distributed/Finalized have
     values: block clearing it, or allow and keep values, or other.
  3. Other paths that auto-set Distributed/Finalized (e.g. email/memo):
     apply the same rule or leave unchanged.

PHASE 2: PLAN
- Smallest generic change. One rule, defined once per layer (frontend
  helper + backend validation), reused — no duplicated logic.
- No schema change.

PHASE 3: IMPLEMENT — touch only what this task needs.

PHASE 4: VERIFY + REPORT — build backend and frontend.

Report:
1. Root cause / what you found (file:line), plus other paths found
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen behaviour: before vs after (read mode, edit mode, entering /
   clearing Mgr Approval)
5. NULL / edge cases: all three NULL; approval set; approval cleared with
   distributed/finalized set; existing inconsistent rows; cancelled
   review; locked/finalized review; API call bypassing the UI
6. Other screens/reports checked and unaffected (status tiles, Review
   Progress buckets, reports filtering by these dates)
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.

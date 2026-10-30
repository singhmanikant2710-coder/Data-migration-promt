#244 Record Retention – DEV testing PASSED (preview + guards A/B/C, cutoff boundaries, rollback via fault injection, successful delete with exact per-table counts and audit, 409 ELIGIBILITY_CHANGED with Aborted audit row, typed DELETE). Make ONLY these small changes:

1. REMOVE the test fault hook completely: RecordRetention:TestFaultAfterStep, TEST_FAULT_INJECTED code path, any related config reads, contracts and comments. It must not ship in a banking app. Keep RecordRetention:CommandTimeoutSeconds.

2. UX after a failed Execute: when Execute returns 409 ELIGIBILITY_CHANGED (or any error), keep the error banner but automatically re-run Preview so the stale eligible table is not shown with an enabled Confirm Deletion button. No other UI changes.

3. Confirmation text: the UI accepts " DELETE" (leading/trailing spaces) – that is fine. Make sure the SERVER applies the same rule: trim, then case-sensitive ordinal compare to "DELETE". Report the exact file:line of the server check.

4. Report only (do NOT modify): git diff --stat and git diff -w --stat for scripts/sql/insert-help-tip-checklist-questions.sql and scripts/sql/insert-samples-type-selections.sql – are the changes whitespace/line-ending only?

5. Confirm git diff of frontend/src/components/Sidebar.tsx is exactly one added line (no reformatting of existing lines).

6. test-data scripts (not committed): the seed script's RAISERROR used DB_NAME() directly as an argument (syntax error). Fix the pattern in seed/verify/cleanup: DECLARE @db sysname = DB_NAME(); placed BEFORE the IF, then IF @db NOT LIKE N'%DEV%' BEGIN RAISERROR(..., @db) ... RETURN; END.

PHASE 4 REPORT (in chat): ADDED and REMOVED/CHANGED with file:line; on-screen before/after for item 2; confirm nothing else changed; dotnet build, dotnet test, npx tsc --noEmit, npm run build results; git status (restore next-env.d.ts and tsconfig.tsbuildinfo if rewritten; leave scripts/sql/find-reviews-with-blanked-ratings.sql alone; test-data/ and devin-output/ not staged). Do not commit or push.


7. UI consistency – table headers on the Record Retention page: the column headers in "Samples to delete in this run", "Excluded samples" and "Run History" are grey. They must look exactly like the other screens' tables (e.g. Maintenance > CAS Findings: navy header bar with white, bold, uppercase header text).
   - First find how CAS Findings (and other existing tables) style their header – shared component, CSS class or Tailwind classes – and give file:line.
   - REUSE that same existing component/class/tokens in the 3 Record Retention tables. Do not hardcode new colours and do not change the shared style itself (other screens must stay unchanged).
   - Keep the horizontal scroll, the Total row and the right-aligned number columns working.
   - Report: on-screen before/after for the 3 table headers, and confirm no shared file was modified.

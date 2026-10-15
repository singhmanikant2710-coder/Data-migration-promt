UAT #235 – Policy Exceptions: remove the auto-added "E00 - No Policy Exceptions" row in Edit mode; work like the Covenants tab.

Geoff's issue: a new review shows zero PE rows in non-edit mode, but clicking Edit auto-shows an "E00 - No Policy Exceptions" row. It is not saved unless the user picks another value and then reselects E00. Required: no auto row. The user must click "+ Add Policy Exception" to create a row, then select E00 (or any type), and that saves normally – same as Covenants.

PHASE 1 – Read-only discovery (file:line, no edits):
1. Where is the E00 row auto-added (PolicyExceptionsSection.tsx ~81-93 / usePolicyExceptions.ts)? Why is it not saved (not marked dirty/_new)?
2. How does the Covenants tab handle "+ Add" and first selection? What makes a newly added row get saved?
3. Does anything else depend on the auto E00 row: validation before Mgr Approval/finalize, PDFs (CAS Linesheet, memos), reports, row-count checks, the PE notes/mitigation defaults from the #233 change (page.tsx ~921-942)?
4. Existing reviews that already have saved PE rows (incl. saved E00): confirm they are unaffected.

PHASE 2 – Plan the smallest generic change: remove only the auto-add; "+ Add Policy Exception" creates a blank "Select..." row; selecting ANY type including E00 from blank must save on first selection. No backend/SQL change unless Phase 1 proves one is needed (then idempotent scripts/sql/ only).

PHASE 3 – Implement only what Phase 2 needs. Do not touch the #233 changes.

PHASE 4 – Verify and report:
- Root cause (file:line).
- ADDED and REMOVED/CHANGED (file:line).
- On-screen before/after (non-edit, edit, after save + refresh).
- NULL/edge cases: review with no PE rows; add a row but leave it "Select..." and Save; add E00 then cancel; existing E00 + other types together; delete the only row.
- Other tabs/PDFs/reports unaffected.
- tsc + frontend build + backend build (if touched), git status.
Do not commit or push.

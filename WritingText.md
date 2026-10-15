UAT #235 follow-up – deleting an unsaved Policy Exception row calls the server and fails.

Repro: review with no PE rows -> Edit -> + Add Policy Exception -> pick E01 -> click Delete. Console: "Failed to clear review policy exception: {}" at PolicyExceptionsSection.tsx:317 (handleDeleteConfirm).

PHASE 1 – Read-only (file:line):
1. Is this pre-existing (before the #235 change)? Show the delete path in handleDeleteConfirm and which endpoint it calls.
2. Why is the server called for a row marked new/unsaved? What does the server return for a code that is not stored, and why is err logged as {}?
3. For a SAVED row, if the server delete fails, is the row still removed from the UI? Does the user see any message?

PHASE 2/3 – Smallest generic fix:
- Unsaved (new) rows: remove locally only, no server call, and drop any pending change for that row so nothing is inserted on Save.
- Saved rows: keep the existing immediate server delete. If it fails, keep the row on screen and show the app's existing error message/toast pattern (no new UI pattern). Log the real error details instead of {}.
- Do not touch the #233 code in page.tsx or the #235 lock/hint.

PHASE 4 – Report: root cause, ADDED and REMOVED/CHANGED (file:line), on-screen before/after, edge cases (delete unsaved row; delete saved row success; delete saved row when server fails; delete the only row -> fields lock + hint; add -> delete -> add again -> Save), tsc + build, git status.
Do not commit or push.

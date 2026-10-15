UAT #235 follow-up – edit a SAVED Policy Exception row, delete it, then Save.

PHASE 1 – Read-only (file:line):
1. Repro by code: review with saved E01 and E03 -> Edit -> change E01 Identified -> delete E01 (server delete succeeds) -> Save. What exactly does page.tsx send for E01, and what does UpsertPolicyExceptionsAsync do when the row no longer exists – update 0 rows, or INSERT it again (row comes back)?
2. Same question when the edit on E01 was only notes/mitigation (review-level fields).

PHASE 2/3 – Only if the row can come back:
- After a successful server delete of a saved row, drop that row's row-level changes (type, category, identified) so Save never re-inserts it.
- Do not lose review-level notes/mitigation/varianceMitigation edits: if other rows remain, keep those values going through the #233 logic (page.tsx ~921-942); if no rows remain, the fields are locked by #235, so confirm what happens to an edit made before the delete.
- Smallest change, no backend change unless Phase 1 proves the backend is the only safe place.

PHASE 4 – Report: root cause, ADDED and REMOVED/CHANGED (file:line), on-screen before/after, edge cases (edit+delete saved row; delete saved row without edit; notes edited on the deleted row while another row remains; delete the last row after editing notes), tsc + build, git status.
Do not commit or push.

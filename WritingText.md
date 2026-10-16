UAT #235 follow-up – three issues found in testing (Policy Exceptions notes/mitigation are ONE review-level value, not per row).

Repro: review with a SAVED E01 (Identified = Yes) and saved notes "Test Notes E01 - Identified - Yes" -> Edit -> + Add -> pick E01 again (Identified = No).

1. New row shows blank notes/mitigation: when "+ Add Policy Exception" creates a row, the notes editor and Exception Mitigation show blank/default instead of the review's current notes/mitigation. If the user types there and saves, the review's existing notes are replaced entirely. Fix: a new row must show the review's CURRENT notes, mitigation and varianceMitigation – the latest edited value in this session if any, otherwise the stored value. Only when the review has nothing stored keep today's defaults ("" / "Not Mitigated"). This must not mark the new row's notes/mitigation as edited, so the #233 logic in page.tsx (~921-942) does not send them unless the user changes them.

2. Typed notes lost on deleting an unsaved row: when an UNSAVED row is deleted and other rows remain, move its notes/mitigation/varianceMitigation edits to a remaining row exactly like the saved-row helper does (reuse it; never overwrite a value already recorded on that row). If no rows remain, keep current behaviour (fields lock, edit dropped).

3. Duplicate types allowed: in the Policy Exception Description dropdown, a type already used by another visible (non-deleted) row on this review must not be selectable (disabled or hidden; follow any existing pattern, e.g. Covenants). The row's own current value stays selectable. Existing saved duplicates must still load and display as before – no data change.
   Read-only first: does Covenants or another dropdown already block duplicates (file:line)? Does the backend reject duplicates?

Do not touch page.tsx (#233) or the #235 lock/hint.

Report: root cause for each, ADDED and REMOVED/CHANGED (file:line), on-screen before/after, edge cases (add row on a review with saved notes -> notes visible; edit notes on row A, add row B -> B shows the edited notes; delete unsaved row with notes while a saved row remains; delete the last unsaved row; change a row's type -> original type available again; deleted row's type available again; review with existing saved duplicates; brand-new review with nothing stored), tsc + build, git status.
Do not commit or push.

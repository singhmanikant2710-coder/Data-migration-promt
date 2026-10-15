UAT #235 follow-up – mitigation/notes when no rows exist.

1. Read-only first: on a review with zero policy exception rows, in Edit mode, can the user change Exception Mitigation and type in Notes? If yes, what happens on Save – saved, rejected, or silently dropped (file:line)?
2. If they are editable but not saved, make the smallest change: when there are no non-deleted rows, show both fields disabled/read-only with a short hint like "Add a policy exception to enter mitigation and notes." Enable them as soon as a row is added. Do not change anything else, and do not touch the #233 code in page.tsx.
3. Report: ADDED and REMOVED/CHANGED (file:line), on-screen before/after, edge cases (add a row then delete it before saving; existing review with rows; non-edit mode), tsc + build, git status.
Do not commit or push.

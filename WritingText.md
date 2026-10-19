UAT #233/#235 follow-up – stored PE notes/mitigation wiped when a review has no PE rows.

Case: Policy_exception_information / Policy_exception_mitigation / Policy_variance_mitigation are stored on the review, but the review has zero PE rows (all rows deleted, or migrated data). The GET only copies these values onto rows (SqlReviewRepository.cs ~4447), so with zero rows the frontend never receives them. "+ Add" shows empty notes, and on first Save page.tsx (~921-942) treats the stored values as blank and sends "" / "Not Mitigated", overwriting the stored data.

PHASE 1 – Read-only (file:line): confirm this path end to end. Which existing reviews in Dev would be affected (give a read-only SQL in your report, do not save it as a script)?

PHASE 2 – Smallest generic fix, rule: stored non-empty notes/mitigation/varianceMitigation must NEVER be overwritten by a default; defaults are only for a review with nothing stored.
Preferred: return these three review-level values in the Policy Exceptions GET independent of rows (e.g. a review-level object), and use them in the frontend as the "stored" value: in page.tsx (#233 logic) instead of baseItems[0], and in the "+ Add" helper in PolicyExceptionsSection.tsx so the new row shows them. If you find a smaller safe option, explain why it is safe.
No schema change.

PHASE 3 – Implement only that. Keep the #233 and #235 behaviour otherwise unchanged.

PHASE 4 – Report: root cause, ADDED and REMOVED/CHANGED (file:line), on-screen before/after, edge cases (zero rows + stored notes -> + Add shows stored notes, Save keeps them; zero rows + nothing stored -> defaults saved as before; rows exist -> unchanged; user edits notes -> edit saved; stored mitigation NULL vs value), backend build + tsc + frontend build, git status.
Do not commit or push.

UAT #251 – Review Progress: when the "Borrowers Sampled" box (bucket "All Status") is selected, the date column header reads "Completed", but the column shows each row's own status date (e.g. Cancelled 10/8/2026, Approved 10/16/2026). It should read "Status Date".

PHASE 1 – Read-only (file:line):
1. Where is the date column header chosen per bucket (review-status/page.tsx ~94-101)? List the header text for every bucket/box: Borrowers Sampled / All Status, Unopened/Cancelled, In Progress, Draft Completed, Approved, Distributed, Finalized.
2. For the All Status bucket, which date does each row show for each status (SqlReviewStatusRepository.cs)? Confirm "Status Date" is an accurate label for all of them.
3. Does the same label appear anywhere else for this view (sort label, tooltip, export, PDF)?

PHASE 2/3 – Smallest change: header = "Status Date" for the All Status bucket only. Every other bucket keeps its current header. No data, SQL or sorting change.

PHASE 4 – Report: ADDED and REMOVED/CHANGED (file:line), on-screen header before/after for every bucket, edge cases (switching between boxes and the Bucket dropdown keeps the header in sync; rows with no date show blank; sorting by this column still works), tsc + frontend build, git status (restore next-env.d.ts / tsconfig.tsbuildinfo if the build rewrites them).
Do not commit or push.

Bug 204 fix — Review Progress "Unopened/Cancelled" bucket should show header "Cancelled" and the Cancelled date from CORE Reviews. Follow the EXISTING pattern the other 5 buckets use (reuse the Completed field; per-bucket label switch). Show all diffs before applying. Do NOT add a new DTO/type field. Do NOT touch other buckets.

FILE 1 — Backend: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs
In GetUnopenedOrCancelledAsync only:
1. Add r.[Cancelled_date] to the SELECT list (append it so existing column ordinals stay stable — StatusLabel currently read at index 12; append Cancelled_date as index 13).
2. Replace the hardcoded `Completed = ""` (line ~664) with the formatted Cancelled_date, using the SAME pattern the other buckets use:
   Completed = cancelledDate.HasValue ? cancelledDate.Value.ToString("M/d/yyyy", us) : ""
   Read cancelledDate from the new ordinal (index 13), mirroring how the other queries read their date column. Rows that are Unopened (not cancelled) will have null Cancelled_date → blank cell (correct, matches this mixed bucket).
Do NOT change the DTO (ReviewStatus.cs) — keep reusing the existing Completed field, exactly like the other 5 buckets.

FILE 2 — Frontend: frontend/src/app/review-status/page.tsx
In the completedColLabel logic (lines ~78-84), add an arm:
   selectedBucket === "Unopened/Cancelled" ? "Cancelled" : ...
so the header reads "Cancelled" for that bucket (keeping the existing labels for all other buckets). Do NOT change the cell rendering (it already renders r.completed).

Do NOT change other bucket queries, the DTO/types, sorting, or backend contract shape.
List every file + line changed. Commit: "Fix Bug 204: Review Progress Unopened/Cancelled bucket shows 'Cancelled' header and Cancelled_date".

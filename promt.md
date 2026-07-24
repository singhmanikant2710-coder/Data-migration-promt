Read-only. No edits. No plan. Just report with file paths + exact code. Do NOT modify anyone's work (including Jothi's).

Context (UAT #157): On the Review Status page, the same review appears under multiple status buckets (e.g. review 21664 shows under both "Draft Completed" and "Finalized"), which inflates the sub-totals. The client says the status workflow should be sequential — Unopened > In Progress > Draft Completed > Approved > Distributed > Finalized — and a review should appear under only ONE status. He notes this works correctly on the Review Queue page, which uses essentially the same data but is filtered to the current user.

Report:
1) How does the Review Queue derive a single status per row? In frontend/src/app/review-queue/page.tsx show getRowStatus() (or equivalent) and any backend status expression in SqlReviewRepository.GetQueueRowsAsync. Paste the exact logic that resolves ONE status per review.
2) How does Review Status currently produce its buckets? In SqlReviewStatusRepository.cs, paste the WHERE predicate of each of the six bucket methods (GetUnopenedOrCancelledAsync, GetInProgressAsync, GetCompletedDraftsAsync, GetApprovedAsync, GetDistributedAsync, GetFinalizedAsync) as they exist right now.
3) Confirm whether those predicates are currently INDEPENDENT (a review satisfying several milestones appears in several buckets) or MUTUALLY EXCLUSIVE.
4) On the Review Status page, are the tile counts and the grid rows both derived from these same six datasets? Show where the counts come from.
5) State exactly what would change, and in how many files, to make Review Status behave like Review Queue: each review resolved to a single status using the sequential precedence Finalized > Distributed > Approved > Draft Completed > In Progress > Unopened/Cancelled, with the tile counts following the same rule.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing.

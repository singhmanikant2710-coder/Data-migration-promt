No problem at all, Geoffrey — good to have it confirmed. I'll switch Review Status to single-status precedence so each review appears under exactly one status and the sub-totals are exact.
On the unlock behaviour — that makes sense. Just to confirm the scope before I build it: when a Manager or Director unlocks a review via the "General" unlock, we currently clear the MGR Approval date. You'd like us to also clear the Finalized date at the same time, so the review falls back to Distributed until it's re-approved and the reviewer enters a new Finalized date.
Two quick questions:
Should the Distributed date be left untouched (so it genuinely falls back to Distributed), or cleared as well?
Should this apply to all three unlock reasons (General, Reconsideration, Appeal), or only the General unlock?
I'll raise this as its own item so it's tracked separately from #157.
Thanks, Manikant



Backend only. Single file: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs
Use LIVE DB, ignore columns.csv. Do NOT modify or revert any existing logic authored by anyone (including Jothi). Do NOT change any SELECT list, JOIN, ORDER BY, or the statusCounts construction. Do not plan. Just apply.

UAT #157 (client confirmed): On Review Status a review can currently appear under multiple buckets (e.g. review 21664 shows as both Draft Completed and Finalized), inflating the tile sub-totals. Make the buckets MUTUALLY EXCLUSIVE using the sequential precedence: Finalized > Distributed > Approved > Draft Completed > In Progress > Unopened/Cancelled. This matches how Review Queue already resolves a single status per row.

Change ONLY the WHERE predicates of these methods, exactly as below:

1) GetFinalizedAsync — no change (highest precedence).

2) GetDistributedAsync — no change (already excludes Finalized).

3) GetApprovedAsync — ADD one line excluding Finalized:
     AND r.[Review_finalized_date] IS NULL

4) GetCompletedDraftsAsync — ADD three lines:
     AND r.[Review_distributed_date] IS NULL
     AND r.[Review_finalized_date] IS NULL
     AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
   (it already has AND r.[Review_approval_date] IS NULL — keep it)

5) GetInProgressAsync — no change (already excludes Completed and Cancelled).

6) GetUnopenedOrCancelledAsync — ADD four lines so it sits at lowest precedence:
     AND r.[Completed_date] IS NULL
     AND r.[Review_approval_date] IS NULL
     AND r.[Review_distributed_date] IS NULL
     AND r.[Review_finalized_date] IS NULL
   (keep the existing AND (r.[Start_date] IS NULL OR r.[Cancelled] = 1))

Keep every existing predicate line, the @sampleId filter, the s.[Closed] = 0 join condition, and each method's own date-range filter exactly as they are.

Note: after this change the tile counts will no longer overlap and WILL sum to the total. That is the intended behaviour for this ticket.

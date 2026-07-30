Single-file edit only:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs

ROOT CAUSE (confirmed via SQL): Each Review Status bucket applies a 
@startDate/@endDate range filter on its milestone date column. Review Queue 
applies NO such date filter. This drops boundary reviews (whose milestone 
date falls outside the sample window) from Status buckets, causing counts 
to be short by 1 in In Progress, Distributed, Finalized, etc. Verified: 
In Progress for Sample 354 returns 15 without the date range, 14 with it.

FIX: Remove the @startDate/@endDate WHERE predicates from ALL bucket 
queries so Review Status classifies reviews the same way Review Queue does 
(by milestone-date NULL/NOT-NULL state only, never by a date window).

In each of these methods, delete the two lines matching this pattern from 
the WHERE clause (the column name varies per bucket):

    AND (@startDate IS NULL OR r.[<DateColumn>] >= @startDate)
    AND (@endDate IS NULL OR r.[<DateColumn>] < DATEADD(day, 1, @endDate))

Methods and their date columns:
- GetInProgressAsync      -> Start_date
- GetCompletedDraftsAsync -> Completed_date
- GetApprovedAsync        -> Review_approval_date
- GetDistributedAsync     -> Review_distributed_date
- GetFinalizedAsync       -> Review_finalized_date
(GetUnopenedOrCancelledAsync has no date-range predicate — leave as-is.)

CONSTRAINTS:
- Remove ONLY the two date-range predicate lines per method. Do NOT touch 
  the milestone-date NULL/NOT-NULL conditions, Cancelled conditions, 
  s.Closed = 0, @sampleId filter, SELECT lists, JOINs, or ORDER BY.
- If @startDate/@endDate parameters become unused, leave the parameters in 
  the method signature (do NOT change signatures or callers) to keep this 
  a single-file change. Only their WHERE usage is removed.
- Do NOT touch any other file, the frontend, the Review Queue repository, 
  or the Borrowers Sampled query.
- Do NOT revert the prior frontend byDate fix.

After edit, list exactly which lines were removed in each method.

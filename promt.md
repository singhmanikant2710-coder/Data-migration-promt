Backend only. File: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs
Do not read other files. Do not plan. Do not explain. Just apply these SQL WHERE-clause edits.

1) GetInProgressAsync — delete these lines from the WHERE clause:
     AND r.Review_distributed_date IS NULL
     AND r.Completed_date IS NULL
     AND r.Review_finalized_date IS NULL
     AND r.Review_approval_date IS NULL
     AND (r.Cancelled IS NULL OR r.Cancelled = 0)
   Keep only: r.Start_date IS NOT NULL   (plus the Samples join, @sampleId filter, Start_date range filter)

2) GetFinalizedAsync — delete:
     AND r.Review_approval_date IS NULL
     AND (r.Cancelled IS NULL OR r.Cancelled = 0)
   Keep only: r.Review_finalized_date IS NOT NULL

3) GetUnopenedOrCancelledAsync — replace the entire predicate with exactly:
     (r.Start_date IS NULL OR r.Cancelled = 1)
   Remove the checks on Review_distributed_date, Completed_date, Review_finalized_date, Review_approval_date.

4) GetApprovedAsync — delete (r.Cancelled IS NULL OR r.Cancelled = 0) if present.
     Keep only: r.Review_approval_date IS NOT NULL

5) GetDistributedAsync — delete these if present:
     AND r.Completed_date IS NULL
     AND r.Review_finalized_date IS NULL
     AND r.Review_approval_date IS NULL
     AND (r.Cancelled IS NULL OR r.Cancelled = 0)
   Keep only: r.Review_distributed_date IS NOT NULL

6) GetCompletedDraftsAsync — leave as-is (already fixed).

In every helper keep: INNER JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id AND s.Closed = 0, the (@sampleId IS NULL OR r.Sample_id = @sampleId) filter, and that helper's own date-range filter (Unopened/Cancelled has none).

Do NOT change borrowersSampled. Do NOT change the statusCounts list. Do NOT touch any other file.

Expected after fix (Select All, no date filter): 264 / 112 / 158 / 136 / 118 / 0 / 116

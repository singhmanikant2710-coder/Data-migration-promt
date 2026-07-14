Backend only, single file: SqlReviewStatusRepository.cs. Do not read other files. Do not plan. Just apply the edits.

Your previous change fixed GetCompletedDraftsAsync only. Three helpers still contain the old exclusion clauses and are returning wrong counts. Fix them now.

1) GetInProgressAsync — currently returns 16, must return 158.
   Delete these lines from its WHERE clause:
     AND r.Review_distributed_date IS NULL
     AND r.Completed_date IS NULL
     AND r.Review_finalized_date IS NULL
     AND r.Review_approval_date IS NULL
     AND (r.Cancelled IS NULL OR r.Cancelled = 0)
   The only remaining condition (besides the Samples join, the @sampleId filter, and the Start_date date-range filter) must be:
     r.Start_date IS NOT NULL

2) GetFinalizedAsync — currently returns 1, must return 116.
   Delete:
     AND r.Review_approval_date IS NULL
     AND (r.Cancelled IS NULL OR r.Cancelled = 0)
   Only remaining condition:
     r.Review_finalized_date IS NOT NULL

3) GetUnopenedOrCancelledAsync — currently returns 109, must return 112.
   It currently requires ALL milestone columns to be NULL. Replace the entire predicate with exactly:
     (r.Start_date IS NULL OR r.Cancelled = 1)
   Remove the checks on Review_distributed_date, Completed_date, Review_finalized_date, Review_approval_date.

4) GetApprovedAsync and GetDistributedAsync — remove the (r.Cancelled IS NULL OR r.Cancelled = 0) clause if it is still present. Their only conditions must be r.Review_approval_date IS NOT NULL and r.Review_distributed_date IS NOT NULL respectively.

Keep in every helper: the INNER JOIN to dbo.[02_CORE_01_Samples] on s.Closed = 0, the (@sampleId IS NULL OR r.Sample_id = @sampleId) filter, and each helper's own date-range filter (except Unopened/Cancelled, which has none).

Target values with no date filter, Select All:
  264 / 112 / 158 / 136 / 118 / 0 / 116
Sample_id 311:
  21 / 10 / 12 / 11 / 11 / 0 / 11
The buckets overlap and will NOT sum to the total. That is correct.

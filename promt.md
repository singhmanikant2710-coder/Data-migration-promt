Task: Backend fix in SqlReviewStatusRepository.cs — GetStatusPageAsync and its six helper methods. Single file only. Use LIVE DB, ignore columns.csv. Do not touch the frontend. Just apply, do not re-plan.

The buckets are currently MUTUALLY EXCLUSIVE (each helper excludes rows that have a "later" milestone). The client's spec is OVERLAPPING — a review that is Approved still has Start_date NOT NULL and must be counted in BOTH "In Progress" AND "Approved". Fix as follows.

--- 1) Remove the exclusion clauses from each helper's WHERE ---
Keep the join to Samples (s.Closed = 0), keep the @sampleId filter, keep the date-range filter on each helper's own milestone column. Only remove the "later milestone IS NULL" exclusions and the Cancelled exclusion. New predicates, exactly:

  GetInProgressAsync        : r.Start_date IS NOT NULL
  GetCompletedDraftsAsync   : r.Completed_date IS NOT NULL
  GetApprovedAsync          : r.Review_approval_date IS NOT NULL
  GetDistributedAsync       : r.Review_distributed_date IS NOT NULL
  GetFinalizedAsync         : r.Review_finalized_date IS NOT NULL
  GetUnopenedOrCancelledAsync : r.Start_date IS NULL OR r.Cancelled = 1     (no date filter — leave as-is)

Note GetUnopenedOrCancelledAsync currently requires ALL milestones to be NULL. Replace that with the single condition above: Start_date IS NULL OR Cancelled = 1.

--- 2) Fix BorrowersSampled ---
It is currently computed as statusCounts.Sum(x => x.Count). That is WRONG — the buckets now overlap and must not sum to the total.
Replace it with an independent count: the total number of in-scope reviews, i.e. COUNT(*) over dbo.[02_CORE_02_Reviews] r INNER JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id AND s.Closed = 0, with the same (@sampleId IS NULL OR r.Sample_id = @sampleId) filter and NO date filter. Add a small helper for this if needed.

--- 3) Dead code ---
The unused EF "agg" aggregation in GetStatusPageAsync is not used in the response. Remove it.

--- 4) Grid parity ---
The grid datasets and the tile counts share the same helpers, so after this change the Bucket filter will correctly return the overlapping row sets (e.g. Bucket = "In Progress" returns all rows where Start_date IS NOT NULL, including Approved and Finalized ones). This is intended. Do not add exclusions back to keep the grid "clean".

--- 5) Expected values (already verified against the live DB — the API must return exactly these, with no date filter applied) ---
  Select All (all reviews joined to samples where Closed = 0):
    BorrowersSampled 264 | Unopened/Cancelled 112 | In Progress 158 | Draft Completed 136 | Approved 118 | Distributed 0 | Finalized 116
  Sample_id = 311:
    21 | 10 | 12 | 11 | 11 | 0 | 11

These will NOT sum to the total. That is correct — do not "fix" it.

After the edit, run read-only diagnostics on this file only and report any errors.

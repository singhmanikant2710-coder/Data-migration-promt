Single-file edit only: 
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs

GOAL: Make the six Review Status bucket predicates exactly match Review 
Queue's classification logic. Review Queue is the source of truth. Two 
defects to fix:

DEFECT 1 — Cancelled NULL handling:
Review Queue treats "not cancelled" as Cancelled != 1 (NULL and 0 both 
pass). Review Status uses strict r.Cancelled = 0, which drops rows where 
Cancelled IS NULL — they fall into no bucket and vanish.

FIX: In EVERY bucket query where the WHERE clause contains
    AND r.[Cancelled] = 0
replace it with
    AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)

Apply this to: In Progress, Draft Completed, Approved, Distributed, 
Finalized. (Unopened/Cancelled uses Cancelled = 1 in its OR predicate — 
leave that one as-is, but see Defect 1b below.)

DEFECT 1b — Unopened/Cancelled must also catch NULL-cancelled unopened rows 
consistently. Its current predicate:
    AND (r.[Start_date] IS NULL OR r.[Cancelled] = 1)
Leave this logic unchanged (Start_date IS NULL already catches unopened; 
Cancelled = 1 catches cancelled). Do not modify.

DEFECT 2 — Distributed and Finalized must exclude rows that Review Queue 
classifies as Approved. Review Queue is mutually exclusive: any row with 
Review_approval_date set goes to Approved only.

Match Queue's exact predicates:

Queue Approved   = Cancelled != 1 AND Review_approval_date IS NOT NULL
Queue Distributed= Cancelled != 1 AND Review_distributed_date IS NOT NULL 
                   AND Completed_date IS NULL AND Review_finalized_date IS NULL 
                   AND Review_approval_date IS NULL
Queue Finalized  = Cancelled != 1 AND Review_finalized_date IS NOT NULL 
                   AND Review_approval_date IS NULL

FIX for Distributed (GetDistributedAsync) WHERE clause — ensure it reads:
    AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
    AND r.[Review_distributed_date] IS NOT NULL
    AND r.[Review_finalized_date] IS NULL
    AND r.[Review_approval_date] IS NULL
    AND r.[Completed_date] IS NULL
(add the Review_approval_date IS NULL and Completed_date IS NULL guards)

FIX for Finalized (GetFinalizedAsync) WHERE clause — ensure it reads:
    AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
    AND r.[Review_finalized_date] IS NOT NULL
    AND r.[Review_approval_date] IS NULL
(add the Review_approval_date IS NULL guard)

CONSTRAINTS:
- Edit ONLY the WHERE predicates described above. Do NOT change SELECT lists, 
  JOINs, ORDER BY, date-range (@startDate/@endDate) predicates, or method 
  signatures.
- Do NOT touch any other file, the frontend, or the Borrowers Sampled query.
- Do NOT revert or alter the byDate/frontend changes from the prior fix.
- Keep all existing WITH (NOLOCK) hints and parameter usage intact.

After edit, list exactly which lines changed in which methods.



SELECT
  SUM(CASE WHEN Review_approval_date IS NOT NULL 
           AND Review_distributed_date IS NOT NULL 
           AND Review_finalized_date IS NULL THEN 1 ELSE 0 END) AS Approved_and_Distributed,
  SUM(CASE WHEN Review_approval_date IS NOT NULL 
           AND Review_finalized_date IS NOT NULL THEN 1 ELSE 0 END) AS Approved_and_Finalized,
  SUM(CASE WHEN Cancelled IS NULL THEN 1 ELSE 0 END) AS Cancelled_is_null
FROM dbo.[02_CORE_02_Reviews]
WHERE Sample_id = @sample354Id;

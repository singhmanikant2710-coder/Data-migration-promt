File to modify: SqlReviewStatusRepository.cs (ONLY this file)

The Review Status count buckets are returning wrong numbers vs the database. 
SQL-verified correct values: Approved=4974 (OK), Finalized=0 (OK), 
Distributed=0 (OK), Draft Completed=118, In Progress=33, total non-cancelled=5173.
But the API returns: borrowersSampled=6839, In Progress=523, Draft Completed=119 
(all wrong). Fix the bucket logic.

FIX 1 — UnopenedOrCancelled double-counts. Current:
UnopenedOrCancelled = g.Sum(r =>
    (r.Start_date == null && r.Review_distributed_date == null &&
     r.Completed_date == null && r.Review_finalized_date == null &&
     r.Review_approval_date == null ? 1 : 0) +
    (r.Cancelled == true ? 1 : 0))
A review that is both cancelled AND has all dates null is counted twice. 
Change the "+" of two conditions into a single OR so each review counts once:
UnopenedOrCancelled = g.Sum(r =>
    ( (r.Start_date == null && r.Review_distributed_date == null &&
       r.Completed_date == null && r.Review_finalized_date == null &&
       r.Review_approval_date == null)
      || r.Cancelled == true )
    ? 1 : 0)

FIX 2 — In Progress returns 523 but should be 33. The Review Queue screen 
computes the same "Open" count correctly as 33 using the same intended logic. 
Verify the deployed InProgress condition actually excludes cancelled rows and 
all later-stage dates. The correct condition must be:
InProgress = g.Sum(r =>
    r.Start_date != null &&
    r.Review_distributed_date == null &&
    r.Completed_date == null &&
    r.Review_finalized_date == null &&
    r.Review_approval_date == null &&
    (r.Cancelled == null || r.Cancelled == false)
        ? 1 : 0)
(Add the Cancelled exclusion if it is missing — that is the likely cause of 
523 vs 33: cancelled or already-advanced reviews are leaking into In Progress.)

FIX 3 — Draft Completed returns 119 but SQL says 118 (off by one). Verify the 
condition matches exactly:
DraftCompleted = g.Sum(r =>
    r.Completed_date != null &&
    r.Review_finalized_date == null &&
    r.Review_approval_date == null &&
    (r.Cancelled == null || r.Cancelled == false)
        ? 1 : 0)
(The off-by-one is likely one cancelled review leaking in; add the Cancelled 
exclusion if missing.)

IMPORTANT — also check: is the source queryable (qAll) already filtered to 
exclude Cancelled rows before bucketing, or not? If qAll does NOT pre-filter 
cancelled, then In Progress / Draft Completed / etc. can include cancelled 
reviews, which explains the inflated numbers. Report what qAll's filter is.

Do NOT change Approved, Finalized, or Distributed (they are correct). Do NOT 
change the frontend. Modify ONLY SqlReviewStatusRepository.cs. After editing, 
paste back all changed bucket conditions AND tell me whether qAll pre-filters 
Cancelled.

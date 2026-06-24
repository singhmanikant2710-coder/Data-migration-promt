File to modify: SqlReviewStatusRepository.cs (ONLY this file)

The Review Status counts don't match the database. The Review Queue screen 
(SqlReviewRepository.cs / GetQueuePageAsync) computes the same counts CORRECTLY. 
Make Review Status match Review Queue's approach.

Reference — how Review Queue does it correctly:
- Its base query filters out cancelled BEFORE any counting:
    var q = _db.Reviews.AsNoTracking().Where(r => r.Cancelled != true);
- So NO bucket ever counts cancelled reviews.
- Its "Not Open" bucket counts only genuinely-unopened reviews (all 5 dates 
  null), and since cancelled is already filtered out, it never includes cancelled.

CURRENT PROBLEM in Review Status:
- Its base queryable (qAll) does NOT pre-filter cancelled, so cancelled reviews 
  leak into buckets.
- Its "UnopenedOrCancelled" bucket counts cancelled reviews and adds them into 
  the bucket sum, inflating borrowersSampled (showing 6006 instead of 5173).

FIX (two changes):

CHANGE 1 — Add the cancelled filter to the base queryable, exactly like Queue:
Find where qAll is defined (the base Reviews queryable used for the buckets) and 
add .Where(r => r.Cancelled != true) so cancelled reviews are excluded before 
all bucketing — matching Review Queue.

CHANGE 2 — Fix the UnopenedOrCancelled bucket so it only counts genuinely 
unopened (all 5 dates null), NOT cancelled (cancelled is now already filtered 
out by Change 1). Match Queue's "Not Open" condition:
UnopenedOrCancelled = g.Sum(r =>
    (r.Start_date == null &&
     r.Review_distributed_date == null &&
     r.Completed_date == null &&
     r.Review_finalized_date == null &&
     r.Review_approval_date == null) ? 1 : 0)
(Remove the "+ (r.Cancelled == true ? 1 : 0)" part entirely.)

After these two changes, with cancelled excluded at the source and the unopened 
bucket counting only genuinely-unopened:
- borrowersSampled (sum of buckets) should become 5173 (matching Queue's total)
- In Progress stays 33, Draft Completed stays 118, Approved 4974, Finalized 0, 
  Distributed 0 — all already correct.

NOTE: After this, the "Unopened/Cancelled" tile will show only genuinely-unopened 
count (around 48), no longer including cancelled. If the business wants cancelled 
shown somewhere, that's a separate decision — for now match Queue's behavior 
(cancelled excluded entirely).

Modify ONLY SqlReviewStatusRepository.cs. Do NOT change other buckets' 
conditions (Approved/Finalized/Distributed/DraftCompleted/InProgress). Do NOT 
touch the frontend or Queue. After editing, paste back: the qAll definition 
line and the changed UnopenedOrCancelled condition.

File to modify: SqlReviewStatusRepository.cs (ONLY this file)

CONTEXT: The "Unopened/Cancelled" tile must show BOTH genuinely-unopened reviews 
AND cancelled reviews (the tile name is literally "Unopened/Cancelled"). 
Currently qAll filters out cancelled (.Where(r => r.Cancelled != true)), so the 
tile only shows unopened (~48) and cancelled count is missing.

REQUIRED BEHAVIOR:
- Keep qAll filtering out cancelled (so the OTHER buckets — In Progress, Draft 
  Completed, Approved, Finalized, Distributed — stay correct and never include 
  cancelled). DO NOT remove that filter.
- The "Unopened/Cancelled" tile count = (genuinely-unopened count from the 
  cancelled-filtered qAll) + (separate count of cancelled reviews from the full 
  Reviews table). These two sets never overlap (cancelled rows aren't in qAll), 
  so simple addition is safe — no double-counting.
- borrowersSampled must REMAIN the sum of the active buckets = 5173 (matching 
  Review Queue). The cancelled count must NOT be added into borrowersSampled. 
  Only the Unopened/Cancelled TILE shows the combined number; the grand total 
  (borrowersSampled) stays 5173.

IMPLEMENTATION:
1. Keep the UnopenedOrCancelled bucket (computed over the cancelled-filtered 
   qAll) as the genuinely-unopened count only:
   UnopenedOrCancelled = g.Sum(r =>
     (r.Start_date == null && r.Review_distributed_date == null &&
      r.Completed_date == null && r.Review_finalized_date == null &&
      r.Review_approval_date == null) ? 1 : 0)

2. Compute a SEPARATE cancelled count from the FULL Reviews table (not qAll), 
   respecting the same sampleId filter if one is applied:
   - cancelledCount = count of Reviews where Cancelled == true (AND Sample_id == 
     sampleId if sampleId.HasValue). Use the same parsed-sampleId logic the rest 
     of this method already uses for filtering by sample.

3. When building the statusCounts list, set the "Unopened/Cancelled" item's 
   Count = unopenedCount + cancelledCount (the combined display number).

4. borrowersSampled: keep it as the sum of the ACTIVE buckets only 
   (Approved + Finalized + Distributed + DraftCompleted + InProgress + 
   genuinely-unopened), WITHOUT the cancelled count. So borrowersSampled stays 
   5173. 
   IMPORTANT: if borrowersSampled is currently computed as statusCounts.Sum(...), 
   and the Unopened/Cancelled item now includes cancelled, then summing 
   statusCounts would wrongly re-add cancelled into the total. Prevent this: 
   compute borrowersSampled from the active-bucket counts only (exclude the 
   cancelled portion), OR sum the buckets before adding cancelled to the tile. 
   Make sure borrowersSampled = 5173, not 5173 + cancelled.

CONSTRAINTS:
- Modify ONLY SqlReviewStatusRepository.cs.
- Do NOT change the other bucket conditions (Approved/Finalized/Distributed/
  DraftCompleted/InProgress).
- Do NOT touch the frontend or Review Queue.
- Keep the sampleId filtering behavior consistent (cancelled count must also 
  respect the selected sample).

After editing, paste back:
1. The cancelledCount computation,
2. The Unopened/Cancelled item's Count assignment,
3. The borrowersSampled computation,
so I can verify it shows combined on the tile but keeps total at 5173.

verify in sql cancel count 

SELECT COUNT(*) AS cancelled_total FROM dbo.[02_CORE_02_Reviews] WHERE Cancelled = 1;

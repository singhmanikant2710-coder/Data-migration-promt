File (READ-ONLY, do NOT edit): the Review Queue backend repository/service 
that computes the "Progress Status" counts (Not Open, Open, Draft Completed, 
Approved, Draft Distributed, Finalized, Totals). Likely SqlReviewQueueRepository.cs 
or similar — find it.

Report in plain text only:

1. Paste the EXACT condition/logic for each Progress Status count:
   - Not Open
   - Open
   - Draft Completed
   - Approved
   - Draft Distributed
   - Finalized
2. How is "Totals" (5173) computed? Sum of the buckets, or a separate count? 
   Paste that exact code.
3. Does the source queryable filter out Cancelled rows BEFORE counting? Paste 
   the WHERE / .Where() filter applied to the base query. Specifically: are 
   cancelled reviews excluded entirely, or counted in some bucket?
4. Is there any "Not Open" / unopened bucket, and does it count cancelled rows 
   or only genuinely-unopened (all dates null, not cancelled) rows?

Report only. Do NOT edit anything.

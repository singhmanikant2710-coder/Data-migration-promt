File (READ-ONLY, do NOT edit): SqlReviewStatusRepository.cs

Report in plain text only. The UI Review Status tiles show different counts 
than the database actually has:
- "In Progress" tile shows 523 but SQL says 33
- "Borrowers Sampled" tile shows 6839 but total non-cancelled reviews = 5173, 
  and the sum of all status buckets = 5125

For the count-bucket logic in this repository, paste EXACTLY:
1. The full LINQ/EF condition used for the "In Progress" bucket. What date 
   columns does it check and exclude?
2. How "Borrowers Sampled" (borrowersSampled) is computed — is it the sum of 
   the buckets, a separate COUNT, or something else? Paste that exact code.
3. The conditions for all other buckets (Approved, Finalized, Distributed, 
   Draft Completed) so I can compare them against the SQL.
4. Is there any filter applied differently (e.g. Cancelled handling, or a 
   different table/source) for borrowersSampled vs the buckets?

Report only. No edits.

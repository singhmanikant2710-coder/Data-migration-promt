READ-ONLY. Do not edit anything. Diagnostics only.

Context: Review Queue counts are CORRECT. Review Progress / Review Status 
screen counts are WRONG — individual borrowers are dropping out of status 
buckets and disappearing from search, even though the "Borrowers Sampled" 
total matches. Example: review ID 20120 (American Bankcard Processing LLC) 
appears in Review Queue but is hidden in Review Status.

Compare the two data paths and report back — DO NOT change any code:

1. Review Queue path:
   - backend SqlReviewRepository.GetQueueRowsAsync — show the full SQL/query, 
     all JOINs, and the WHERE clause. Note the join key(s) between Samples 
     and Reviews.

2. Review Status path:
   - backend SqlReviewStatusRepository.cs — show all 6 bucket methods 
     (Unopened/Cancelled, In Progress, Draft Completed, Approved, 
     Distributed, Finalized). For each, show the SQL/query, JOINs, and 
     WHERE conditions that assign a review to that bucket.
   - Also show any base/"get all rows" query the buckets build on.

3. Report specifically:
   a. What join key each path uses between Samples and Reviews 
      (Sample_id vs Sample_number vs Customer_number etc.), and whether 
      they differ.
   b. Whether Review Status uses an INNER JOIN anywhere that Review Queue 
      uses a LEFT JOIN (or vice versa) — an INNER JOIN on a mismatched key 
      would silently drop rows.
   c. The exact bucket conditions for "Distributed" and "Finalized" in 
      Review Status, since those two are off-by-one/off in Geoff's data.
   d. Whether the sum of the 6 Review Status bucket conditions is 
      guaranteed to cover every review (mutually exclusive AND exhaustive), 
      or whether a review can satisfy zero buckets and thus vanish.

Do not edit any file. Output findings only.

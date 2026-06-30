READ-ONLY. Do NOT edit. Report only.

Review Queue shows a real Exposure computed from the Accounts table, but Review 
History and Review Status show TTBA_exposure (often 0). I want History and Status 
to compute exposure the SAME way as Queue.

Report ONLY:

1. In SqlReviewRepository.cs (GetQueuePageAsync), show the EXACT SQL that computes 
   the Accounts-based exposure: the subquery/join to the Accounts table, the SUM 
   column, and how it links by Review_id. Show the full SELECT for the exposure field.

2. In SqlReviewHistoryRepository.cs (GetHistoryRowsAsync), show the current SELECT 
   and exactly which column/expression provides Exposure (TTBA_exposure).

3. In SqlReviewStatusRepository.cs, show the current SELECT and which column/expression 
   provides Exposure.

4. Confirm the exact Accounts table name and the column summed for exposure 
   (e.g. dbo.[02_CORE_04_Accounts] and its Commitment column).

Report only with exact SQL. No edits.

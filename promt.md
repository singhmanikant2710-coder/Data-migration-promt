READ-ONLY. Do NOT edit. Report only.

Review Queue shows a real Exposure value computed from the Accounts table, 
but Review History and Review Status show TTBA_exposure (often 0). 
I need to make History and Status compute exposure the same way as Queue.

Report ONLY:

1. In SqlReviewRepository.cs (GetQueuePageAsync), show the EXACT SQL/subquery 
   that computes the Accounts-based exposure (AccountsCommittedExposure or similar). 
   Show the join/subquery to the Accounts table, the SUM column, and how it links 
   by Review_id.

2. In SqlReviewHistoryRepository.cs (GetHistoryRowsAsync) — show the current SELECT 
   and exactly where/how Exposure is currently pulled (TTBA_exposure).

3. In SqlReviewStatusRepository.cs — same: show the current SELECT and how Exposure 
   is currently pulled.

4. What is the exact Accounts table name and the commitment/exposure column it sums? 
   (e.g. dbo.[02_CORE_04_Accounts].Commitment)

Report only with exact SQL. No edits.

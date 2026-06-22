Found the root cause. The single-review fetch returns HTTP 500 with:
"Invalid column name 'Comp_call_description'."

This is a backend SQL bug, NOT a frontend issue. The SQL query used 
by GetReviewQueueByKeysAsync references a column 'Comp_call_description' 
that does not exist in the database.

Read these files completely. Do NOT modify anything yet:
1. backend/src/Casrr.Application/Services/ReviewService.cs — find 
   GetReviewQueueByKeysAsync
2. backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs 
   — find the method behind GetReviewQueueByKeysAsync and locate the 
   exact SQL query that uses 'Comp_call_description'

Report back:
1. The exact SQL query containing 'Comp_call_description'
2. Which table/alias that column is being selected from
3. The exact line and surrounding SELECT columns

Also: does the working review-queue LIST query (GetQueueRowsAsync) 
use this same column or a different one? Compare the two.

Do NOT edit anything. Just report.

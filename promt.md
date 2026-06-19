Read these files completely first, do not modify anything:
1. Find and open the file that defines IReviewService interface 
   (search for "interface IReviewService")
2. Find and open its implementation class (search for 
   "class ReviewService" or similar that implements IReviewService)

Show me:
1. Full file paths of both files
2. The GetReviewQueuePageAsync method signature in the interface
3. The full implementation of GetReviewQueuePageAsync — what 
   repository/DbContext it uses internally, and what SQL/query 
   logic determines which rows to return
4. Where these are registered in dependency injection 
   (search StartupExtensions.cs for "IReviewService")

Do not modify anything. Just show me this information.


sql 

Read this file completely first, do not modify anything:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Show me:
1. The GetQueuePageAsync method — full implementation
2. The exact SQL query text used (the Dapper SQL string)
3. All column names selected from [02_CORE_02_Reviews] or any 
   joined tables
4. Is there a WHERE clause filter, and does it check any 
   status/finalized/completed column?
5. Does the query check Review_finalized_date anywhere?

Do not modify anything. Just show me this information.

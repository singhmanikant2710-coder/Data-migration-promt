File to modify: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewHistoryRepository.cs
Method: GetHistoryRowsAsync

STEP 1 — First, READ the entire GetHistoryRowsAsync method (including the SQL query). 
Do not edit anything until you understand the query.

STEP 2 — Modify ONLY this method's SQL query:

PROBLEM: The query LEFT JOINs 02_CORE_01_Samples (alias s) to 02_CORE_02_Reviews 
(alias r) ON r.Sample_id = s.Sample_id. This join never matches because the two 
tables use different Sample_id schemes — so s.Sample_name always comes back NULL, 
and the frontend dropdown ends up empty.
Confirmed: 02_CORE_02_Reviews already has its own Sample_name column, and it is 
populated (not NULL) for every finalized review.

FIX (exactly these 3 changes, nothing else):
1. Remove the entire line: 
   "LEFT JOIN dbo.[02_CORE_01_Samples] AS s WITH (NOLOCK) ON r.[Sample_id] = s.[Sample_id]"
2. In the SELECT list, replace "s.[Sample_name] AS SampleName" with 
   "r.[Sample_name] AS SampleName".
3. In the WHERE clause, change the optional sample filter 
   "s.[Sample_name] LIKE '%' + @sampleName + '%'" to 
   "r.[Sample_name] LIKE '%' + @sampleName + '%'".

Keep everything else exactly the same — all r.* columns, the 
Review_finalized_date IS NOT NULL filter, the Cancelled filter, the borrowerName 
Customer_name LIKE filter, and the ORDER BY clause. Do not change the method 
signature, parameters, or mapping logic.

Modify ONLY this one file: SqlReviewHistoryRepository.cs. Do NOT touch any other 
file. If any other file seems to need changes, STOP and ask me first. 
Do not add any new nuget/npm package.

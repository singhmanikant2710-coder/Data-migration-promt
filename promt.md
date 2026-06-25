File to modify: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewHistoryRepository.cs
Method: GetHistoryRowsAsync

The Review History sample dropdown is empty and the "Sample / Review Name" 
column shows no value. Root cause: the query LEFT JOINs 02_CORE_01_Samples (s) 
to 02_CORE_02_Reviews (r) ON r.Sample_id = s.Sample_id, but these two tables 
use different Sample_id schemes so the join never matches and s.Sample_name 
comes back NULL. The Reviews table (02_CORE_02_Reviews) already has its own 
Sample_name column.

FIX (3 changes):
1. Remove the line: 
   LEFT JOIN dbo.[02_CORE_01_Samples] AS s WITH (NOLOCK) ON r.[Sample_id] = s.[Sample_id]
2. In the SELECT list, replace "s.[Sample_name] AS SampleName" with 
   "r.[Sample_name] AS SampleName".
3. In the WHERE clause, change "s.[Sample_name] LIKE '%' + @sampleName + '%'" 
   to "r.[Sample_name] LIKE '%' + @sampleName + '%'".

Keep everything else identical (all r.* columns, finalized filter, cancelled 
filter, borrowerName filter, ORDER BY). Modify ONLY this file. Do not touch any 
other file. After editing, paste back the full changed query.

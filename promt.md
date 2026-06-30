Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

Add a new public method SaveCrmFindingsAsync that saves CRM findings to 
dbo.[02_CORE_07_Findings], following the SAME transaction pattern as the 
existing UpsertChecklistAsync (single connection, BeginTransaction, commit at end).

Behavior (replace-all):
1. Open connection, begin transaction.
2. DELETE all existing rows for the review:
   DELETE FROM dbo.[02_CORE_07_Findings] WHERE [Review_id] = @reviewId;
3. For each finding in the submitted list, INSERT a row. For Finding_category and 
   Finding_description, LOOK THEM UP from dbo.[03_LIBRARY_01_CAS Findings] by 
   Finding_code (use a subquery in the INSERT, or SELECT them first). 
   Columns to insert:
   - [Review_id]              = @reviewId
   - [Finding_CRM_component]  = finding.Component
   - [Finding_code]           = finding.FindingCode
   - [Finding_category]       = (SELECT [Finding_category] FROM dbo.[03_LIBRARY_01_CAS Findings] WHERE [Finding_code] = @code)
   - [Finding_description]    = (SELECT [Finding_description] FROM dbo.[03_LIBRARY_01_CAS Findings] WHERE [Finding_code] = @code)
   - [Finding_level]          = finding.Severity
   - [Finding_comments]       = finding.Comments
   - [Finding_follow_up]      = finding.FollowUp   (bit)
   DO NOT insert [SSMA_TimeStamp] — it is a timestamp column managed by SQL Server.
4. Skip any finding where FindingCode is null/empty (cannot insert without the PK).
5. Commit the transaction. On error, log like UpsertChecklistAsync does.

Define the input parameter type to match how the findings will be passed 
(a list of objects with Component, FindingCode, Severity, Comments, FollowUp). 
If you need a small DTO/record for this, add it in the same file or reuse an 
existing one — but if that requires creating/editing ANOTHER file, STOP and ask me first.

Use parameterized SqlCommands (no string concatenation of values). 
Match the using/namespace style already in this file.

Modify ONLY SqlReviewRepository.cs. If any other file needs changing 
(interface IReviewRepository, DTOs, etc.), STOP and tell me first — do not edit them.

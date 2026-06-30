Modify ONLY this file:
backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

In the SaveCrmFindingsAsync method, change the behavior from "replace-all" to 
"upsert only". Specifically:

1. REMOVE the "DELETE FROM dbo.[02_CORE_07_Findings] WHERE [Review_id] = @reviewId" 
   statement entirely. Save must NEVER delete findings.

2. For each finding in the list, instead of plain INSERT, do an upsert keyed on 
   the composite key (Review_id + Finding_code), exactly like UpsertChecklistAsync does:

   IF EXISTS (SELECT 1 FROM dbo.[02_CORE_07_Findings]
              WHERE [Review_id] = @reviewId AND [Finding_code] = @code)
       UPDATE dbo.[02_CORE_07_Findings]
       SET [Finding_CRM_component] = @component,
           [Finding_category]      = (SELECT TOP(1) [Finding_category] FROM dbo.[03_LIBRARY_01_CAS Findings] WHERE [Finding_code] = @code),
           [Finding_description]    = (SELECT TOP(1) [Finding_description] FROM dbo.[03_LIBRARY_01_CAS Findings] WHERE [Finding_code] = @code),
           [Finding_level]          = @level,
           [Finding_comments]       = @comments,
           [Finding_follow_up]      = @followUp
       WHERE [Review_id] = @reviewId AND [Finding_code] = @code;
   ELSE
       INSERT INTO dbo.[02_CORE_07_Findings]
           ([Review_id],[Finding_CRM_component],[Finding_code],[Finding_category],
            [Finding_description],[Finding_level],[Finding_comments],[Finding_follow_up])
       VALUES
           (@reviewId, @component, @code,
            (SELECT TOP(1) [Finding_category] FROM dbo.[03_LIBRARY_01_CAS Findings] WHERE [Finding_code] = @code),
            (SELECT TOP(1) [Finding_description] FROM dbo.[03_LIBRARY_01_CAS Findings] WHERE [Finding_code] = @code),
            @level, @comments, @followUp);

3. Keep everything else the same: skip findings with null/empty FindingCode, 
   do NOT insert [SSMA_TimeStamp], keep the transaction (BeginTransaction/Commit), 
   keep parameterized commands, keep the existing error logging.

Modify ONLY SqlReviewRepository.cs. If any other file needs changing, STOP and tell me first.

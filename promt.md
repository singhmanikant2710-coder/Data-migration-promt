READ-ONLY. Do NOT edit. Report only.

I need to wire SaveCrmFindingsAsync into the save flow. Report ONLY:

1. In IReviewRepository (the interface for SqlReviewRepository), show how 
   UpsertChecklistAsync or SaveKeyRisksAsync is declared. I'll add a matching 
   declaration for SaveCrmFindingsAsync. Show the exact file path and the 
   existing method signatures style.

2. In ReviewService.SaveAsync, show the FULL existing block for ONE array-based 
   section that calls a repo method (e.g. how Checklist items are extracted from 
   the payload and passed to UpsertChecklistAsync). I want to mirror it for 
   CRM Findings.

3. Confirm the exact type of dto.CrmFindingsAndRatings.Data and how to read the 
   "findings" array out of it into a list of CrmFindingRow (Component, FindingCode, 
   Severity, Comments, FollowUp). Show how other sections parse their JSON arrays.

Report only with exact code. No edits.

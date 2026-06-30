Modify ONLY this file:
backend/src/Casrr.Application/Services/ReviewService.cs

In SaveAsync, add a new CRM Findings block immediately AFTER the Checklist block 
and BEFORE the "var response = new ReviewFormSaveResponse" line.

The block must:
1. Check the section was posted:
   if (dto.CrmFindingsAndRatings is not null && 
       dto.CrmFindingsAndRatings.Change != SectionChangeKind.None)
   {
       ...
   }

2. Inside, read dto.CrmFindingsAndRatings.Data (a JsonElement?). The Data is a 
   single object that contains a "findings" array. Each finding object has 
   properties: component, findingCode, severity, comments, followUp.

3. Use a LOCAL static TryGetPropertyIgnoreCase helper (same style as the other 
   blocks in this method, e.g. the Covenants/PolicyExceptions blocks) to:
   - get the "findings" array from Data
   - for each element in that array, read component, findingCode, severity, 
     comments (strings), and followUp (boolean; treat true/"true"/1 as true)
   - build a List<CrmFindingRow> where each row sets 
     Component, FindingCode, Severity, Comments, FollowUp

4. Skip elements where findingCode is null/empty.

5. Call:
   await _repo.SaveCrmFindingsAsync(resolvedReviewId, findingRows, ct);

6. Wrap parsing in a try/catch consistent with the other blocks; on parse failure 
   log/ignore gracefully (but a successful parse must call SaveCrmFindingsAsync).

Use the existing resolvedReviewId variable. Match the coding/using style already 
in this file. Do NOT redefine CrmFindingRow — it's already imported via 
Casrr.Application.Reviews.Contracts.

Modify ONLY ReviewService.cs. If any other file needs changing, STOP and tell me first.

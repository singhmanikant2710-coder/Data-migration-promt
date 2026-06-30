READ-ONLY. Do NOT edit. Report only.

To write CRM Findings save, report ONLY:

1. Show the FULL source of UpsertChecklistAsync in SqlReviewRepository.cs 
   (complete method — connection, BeginTransaction, the per-item loop, 
   DELETE/UPDATE/INSERT SQL, cmd.Transaction assignment, Commit). 
   I will mirror this exact structure for CRM Findings.

2. What exact JSON does dto.CrmFindingsAndRatings.Data contain? 
   Show the property names per finding row (component, findingCode, category, 
   description, severity/level, comments, followUp) and whether each row has a 
   per-row change flag (Insert/Update/Delete) or clientKey.

3. How does ReviewService.SaveAsync currently handle dto.CrmFindingsAndRatings 
   after the hasChanges check — is there any block for it at all, or is it 
   completely absent (so I need to add a new block)?

Report only with exact code. No edits.

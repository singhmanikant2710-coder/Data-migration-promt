READ-ONLY. Do NOT edit. Report only.

Checklist tab currently shows no data. I need to understand its current state.

Report ONLY:

1. In the backend, how is Checklist data read? Show the method (e.g. 
   GetChecklistSectionAsync in SqlReviewRepository.cs) — which table it reads 
   (dbo.[02_CORE_08_Checklists]?), the SELECT, and how it filters by Review_id.

2. Which frontend component renders the Checklist tab? Show how it fetches/receives 
   the checklist rows and how it maps them.

3. Is there checklist data in the table for a test review? (Just show the read query 
   so I can run it in SSMS.)

4. Save: is UpsertChecklistAsync (already exists in SqlReviewRepository) wired into 
   ReviewService.SaveAsync? Show the Checklist block in SaveAsync — does it call 
   UpsertChecklistAsync? (This confirms whether Checklist save is implemented.)

5. For the client requirements — report current state ONLY (no changes):
   - Is the answer field limited to Yes/No/N/A anywhere?
   - Is comment mandatory-on-No implemented?
   - Is there any lock/unlock logic on tabs after Manager approval?

Report only with exact code, file paths, and SQL. No edits.

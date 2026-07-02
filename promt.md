READ-ONLY code inspection. Do NOT report on database tables or data. Report ONLY 
on TypeScript/C# code that exists in the repo. For each of these 6 items, open the 
relevant file, and answer "IMPLEMENTED" or "NOT IMPLEMENTED" with the code snippet:

1. In backend ReviewService.SaveAsync — is there a line calling 
   _repo.UpsertChecklistAsync(...) with dto.Checklist? Paste it or say NOT IMPLEMENTED.

2. In the frontend Checklist tab component — is the answer field a select limited to 
   Yes / No / N/A? Paste it or say NOT IMPLEMENTED.

3. In the frontend Checklist tab component — is the comment required when answer is 
   "No"? Paste the validation or say NOT IMPLEMENTED.

4. In the frontend Checklist tab component — are only answer and comment editable 
   while other fields are locked? Paste it or say NOT IMPLEMENTED.

5. Anywhere in the review form — is there a lock-after-approval + Manager/Director 
   "Unlock" button? Paste it or say NOT IMPLEMENTED.

6. In any review tab — are the (i) help icons wired to the help-tips feature? 
   Paste it or say NOT IMPLEMENTED.

Answer all 6. Code only, not data/tables.

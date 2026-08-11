READ-ONLY. One-time complete read for the Unlock Review workflow. Do not edit.

Find and show ALL of the following in ONE pass (search the codebase for 
"Unlock", "Review_finalized_date", "Review_approval_date", unlock handler):

1. BACKEND unlock handler/service — the method(s) that process unlocking a 
   review. Show the COMPLETE method(s) verbatim, including:
   - How "General Revisions" updates fields (approval_date transfer to 
     initial_approval_date, clear approval_date, clear finalized_date, 
     Locked=false, approver_name).
   - How "Reconsideration" and "Appeal" are handled — do they call the same 
     update logic or a separate/incomplete path?
   - The exact field assignments (SQL or EF) for all three options.
   - The "first unlock only" detection (how it checks first unlock).
   File path + full method bodies.

2. The DTO/request model for the unlock action — what data comes from the 
   frontend (which option was selected, sub-form data).

3. FRONTEND Unlock modal component — the three radio options and whether any 
   DEFAULT is pre-selected. File path + the relevant JSX.

Show all three (backend handler, DTO, frontend modal) completely in this one 
response so I don't need to re-read. Findings only, no edits.

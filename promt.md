READ-ONLY. Do NOT edit. Report only. This is about CODE that exists, NOT about data 
or tables.

For the Checklist tab, check the actual code and report each item as 
"IMPLEMENTED" or "NOT IMPLEMENTED" with the file path and code snippet:

1. SAVE WIRING: Open ReviewService.SaveAsync. Is there a block that calls 
   _repo.UpsertChecklistAsync(...) using dto.Checklist? Paste that block. 
   (IMPLEMENTED if the call exists, NOT IMPLEMENTED if absent.)

2. ANSWER = Yes/No/N/A ONLY: In the Checklist frontend component, is the answer 
   field a dropdown/select restricted to exactly Yes, No, N/A? Paste it. 
   (IMPLEMENTED or NOT.)

3. COMMENT MANDATORY ON "No": In the Checklist component, is there validation that 
   requires the comment when answer is "No" and keeps it empty otherwise? Paste it. 
   (IMPLEMENTED or NOT.)

4. FIELD LOCKING: In the Checklist component, are only the answer and comment fields 
   editable while other fields stay locked? Paste how editability is controlled. 
   (IMPLEMENTED or NOT.)

5. REVIEW LOCK/UNLOCK: Anywhere in the review form, is there a lock-after-approval 
   plus a Manager/Director "Unlock" button feature? Paste it. (IMPLEMENTED or NOT.)

6. HELP TIPS: Are the (i)/help icons in any review tab connected to the help-tips 
   library (dbo.[03_LIBRARY_06_Help Tips] via the maintenance help-tips feature)? 
   Paste the connection or state NOT IMPLEMENTED.

Do NOT report on table existence or data. Report ONLY on code implementation. 
File paths + code + IMPLEMENTED/NOT for each of the 6 items.

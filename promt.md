READ-ONLY. Do NOT edit. Report only.

The transactional table dbo.[02_CORE_08_Checklists] is empty. I need to find where 
the standard checklist questions come from.

Report ONLY:

1. Is there a LIBRARY/master table for checklist questions (similar to how 
   03_LIBRARY_01_CAS Findings holds finding codes)? Search table names like 
   03_LIBRARY%Checklist%, %Checklist%, or similar. List any table that holds 
   standard/master checklist questions with their guidance.

2. If such a library exists, show its columns and a sample of its rows 
   (the read query so I can run it in SSMS).

3. In the backend, how does GetChecklistSectionAsync build the checklist for a 
   review — does it read only from 02_CORE_08_Checklists (empty), or does it also 
   pull questions from a library/master? Show the exact SELECT.

4. In the maintenance section, is there a Checklist library management screen 
   (like the CAS Findings one at /maintenance/cas-findings)? Check the maintenance 
   routes/menu.

Report only with exact table names, columns, and SQL. No edits.

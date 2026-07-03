READ-ONLY. Do NOT edit. Report only. Live DB, ignore columns.csv.

CRM Ratings UNSAT checkboxes now save AND restore correctly. But the per-component 
rationale COMMENTS save to DB yet do NOT show on reload (the rationale editors are empty).

Report ONLY:

1. In GetCrmFindingsSectionAsync, does the read now capture the 5 *_comments values 
   from 02_CORE_02_Reviews, and are they included in the returned CrmRatings/CrmFindingsSection? 
   Show the CrmRatings model definition — does it have comment fields per component, 
   or only the 5 rating strings?

2. On the frontend, the CRM Ratings tab (useCrmFindings.ts / CrmRatingsSection.tsx) — 
   the per-component rationale editors use local state "rationales" (general, 
   riskRecognition, ...). How is this local rationales state initialized on load? 
   Does it read comments from the backend response, or does it always start empty?

3. What is the response path/field the frontend would read per-component comments from, 
   if the backend included them?

Report only with exact code and file paths. No edits.

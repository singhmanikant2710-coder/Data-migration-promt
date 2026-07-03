READ-ONLY. Do NOT edit. Report only. Live DB, ignore columns.csv.

CRM Ratings save works (data is in DB: Risk_recognition_UNSAT=1, comments saved), 
but after reload the CRM Ratings tab shows unchecked boxes and empty comments — 
the READ is not returning the saved values.

Report ONLY:
1. In GetCrmFindingsSectionAsync (SqlReviewRepository.cs), show the CURRENT code 
   around the ratings — is the interim "rr = sm = uw = cs = la = null;" still there, 
   or was it replaced with an actual SELECT of the *_UNSAT and *_comments columns? 
   Show the exact current code.

2. Does the read now SELECT Risk_recognition_UNSAT, Risk_recognition_comments, etc. 
   from 02_CORE_02_Reviews and map them into the returned ratings? Show it.

3. Does the returned CrmRatings/section include the per-component comments so the 
   frontend rationale editors can display them? Or only the UNSAT rating strings?

4. On the frontend (useCrmFindings.ts / CrmRatingsSection.tsx), how does it read the 
   ratings + comments from the response to set the checkboxes (Unsatisfactory) and 
   the rationale editors? Does it expect comment fields that the backend isn't sending?

Report only with exact code. No edits.

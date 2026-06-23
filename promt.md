On the Review Status screen, add an "All Samples" option to the 
Select Sample / Review Name dropdown. When "All Samples" is 
selected, call getReviewStatusPage WITHOUT a sampleId (pass 
undefined/no param), so the backend returns combined data for all 
samples (the backend already treats null sampleId as "all").

Modify ONLY frontend/src/app/review-status/page.tsx:
- Add an "All Samples" entry at the top of the dropdown options 
  (e.g. value "" or "all").
- When it's selected, call getReviewStatusPage() with no sampleId 
  so the query string has no sampleId param.
- Default the page to "All Samples" on first load so data shows 
  immediately.

Rules:
- Do NOT change the backend, the service file, or any other file.
- Do NOT change how individual samples are passed (that's a separate 
  fix).
- Show me the diff.

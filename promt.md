READ-ONLY. Do NOT edit. Report only.

When saving CRM Findings, the POST /api/v1/reviews/save payload sends:
  crmFindingsAndRatings: { change: "Upsert", data: { findings: { row-123: {followUp: true} } } }

But the backend expects findings to be an ARRAY of full finding objects:
  findings: [ {component, findingCode, severity, comments, followUp}, ... ]

The frontend is sending findings as an OBJECT keyed by row-id, and only the 
changed field per row (not the full finding). Report ONLY:

1. Which file/function builds the crmFindingsAndRatings payload on save? 
   (likely in the FormChangesContext, the save handler in page.tsx, or 
   useCrmFindings hook)

2. Show the exact code that constructs the "findings" value in the payload. 
   Why is it an object keyed by row-id instead of an array? Why only changed fields?

3. Where is the full current list of findings (all rows with all fields: 
   component, findingCode, severity, comments, followUp) available in state, 
   so it could be sent as a complete array instead?

Report only with exact code and file paths. No edits.

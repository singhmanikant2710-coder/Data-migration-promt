Read-only check, no edits.

Trace the SAVE flow end-to-end for the new "scorecard" section:

1. Show me the centralized save handler (handleSave in page.tsx / ReviewInfoContent) — specifically the code that reads FormChangesContext (getMerged / changes) and builds the save request body.

2. Confirm: does it automatically include ANY section present in FormChangesContext, or does it explicitly pick named sections one by one? If it explicitly lists sections, "scorecard" must be added to that list — check whether it is.

3. Confirm the JSON key the frontend sends for this section matches EXACTLY what the backend controller expects (e.g. frontend sends "scorecard": { comments: "..." } and the controller binds a "scorecard" property with a Comments field).

4. Confirm "scorecard" is in the SectionKey union type in FormChangesContext.tsx.

Report with evidence. If anything is missing, tell me exactly what to add. STOP before editing.

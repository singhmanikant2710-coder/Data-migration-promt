Bug 196 — READ-ONLY, no edits. One pass, answer all, STOP.

The reported error is a PRIMARY KEY / UNIQUE constraint violation shown to the user when editing a Reporting-tab selection. UPDATE only sets Tab+Selection, so trace where an INSERT could fire:

1. In maintenance/selections/page.tsx: does handleSave ever call anything OTHER than updateSelection? Search the whole file for: createSelection, addSelection, POST, insert, api.post. Paste any create/POST call and the condition under which it runs.

2. In SelectionsController.cs: is there a POST endpoint for library selections (create)? Paste its route + the repo method it calls (AddAsync/InsertAsync).

3. In SelectionRepository.cs AddAsync/InsertAsync (create method): paste its full SQL. How does it generate Selection_id for a new row? (MAX+1 per section? a global identity? hardcoded?) 

4. Does the "Add Selection" / "__ADD_NEW__" section flow share the same save path as Edit? When the section dropdown value is "" (empty, from __ADD_NEW__), what does handleSave send?

Then STOP.

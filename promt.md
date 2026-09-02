Bug 196 — READ-ONLY, no edits. One pass, STOP.

1. SelectionRepository.cs CreateAsync — paste the FULL method. Specifically: after existsSql runs, if a row EXISTS, does it throw / return early, or does it still proceed to INSERT? Paste that branching logic exactly.

2. Frontend maintenance/selections/page.tsx handleCreateInline (the Add path): how is idNum (selectionId) computed for a NEW selection? Search for how the new Selection_id is derived — MAX+1? user-typed? Paste those lines.

3. Confirm: for Bug 196 the user is on the Reporting tab editing an existing selection's Reporting name. Does clicking Edit → changing text → Save ever route through handleCreateInline or createSelection? Trace handleSave's actual branch with idOrSectionChanged hardcoded false. Yes/No + the deciding lines.

Then STOP.

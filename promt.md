Bug 196 — READ-ONLY, no edits. One pass, answer, STOP.

1. In frontend maintenance/selections/page.tsx, near the save handler (~line 433): what is the 'section' variable bound to? Paste the useState/assignment lines that set 'section' when Edit is clicked and when the section dropdown changes. Is section editable in edit mode?

2. Paste UpdateSelectionLibraryItemDto class definition (all properties).

3. Re-confirm the EXACT SET clause in UpdateAsync SQL — does it ever SET [Section] or [Selection_id]? Paste the full UPDATE statement verbatim.

Then STOP.

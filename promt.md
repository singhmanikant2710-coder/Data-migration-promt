READ-ONLY. Do NOT edit. Report only.

In CrmFindingsAndRatingsSection.tsx and useCrmFindings.ts, I want each field 
change to stage the FULL findings array (not a partial row-keyed object) into 
FormChangesContext, so the save payload sends findings as a complete array.

Report ONLY:
1. Does useCrmFindings expose the full findings array (state.findings) and the 
   updateRow function to the section component? Show exactly what the hook returns.

2. When a field changes in the section, updateRow is called to update hook state. 
   Is the updated full array available synchronously in the same handler, or only 
   after re-render? (i.e. if I call setSection right after updateRow, will I have 
   the new array or the stale one?)

3. Show ONE complete field-change handler in the section (e.g. the followUp/comments 
   handler) start to finish, including the updateRow call and the setSection call, 
   so I can see how to change it to stage the full array.

Report only with exact code. No edits.

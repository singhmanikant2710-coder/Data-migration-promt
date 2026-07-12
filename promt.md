Read-only diagnostic, no edits.

STEP 2 was applied but the behaviour is UNCHANGED: on the CRM Findings tab I add a row and set component/finding code/severity, switch to CRM Ratings without saving, come back — the unsaved row is STILL gone and the saved DB rows are shown.

Investigate and prove (with evidence from the code, not assumptions):

1. `rowsToRender` uses the condition `if (isEditing && Array.isArray(pending))`. Where does `isEditing` come from in this component? After a tab switch and remount, is `isEditing` still TRUE, or does it reset to FALSE? If it resets to false, the pending snapshot is ignored and the table falls back to saved data — which would exactly explain the observed behaviour. Confirm or rule this out.

2. Is `changes.changes.crmFindingsAndRatings.findings` actually populated at the moment the CRM Findings section remounts after coming back from CRM Ratings? Trace whether FormChangesProvider truly survives the section switch (earlier you said router.replace does not unmount the providers — verify that is actually the case at runtime, including whether ReviewDataContext's refetch on section change causes the section subtree to remount and whether that clears anything).

3. Does anything reset the Edit mode / isEditing flag on section change? Show where isEditing is set and cleared.

4. If `isEditing` is the blocker, tell me the correct fix: should rowsToRender use the pending snapshot regardless of isEditing (i.e. drop the isEditing condition), or should Edit mode itself persist across tab switches? Recommend which is correct for this app and why.

Report with evidence. STOP before editing.

Approved — apply the single-file change to CrmFindingsAndRatingsSection.tsx (remove the isEditing gate from rowsToRender).

Additionally, verify one thing that could still break this (report it, don't guess):

Does `changes.setSection(key, payload)` MERGE into the existing section object, or REPLACE it entirely?

This matters because CrmRatingsSection also writes to the SAME section key "crmFindingsAndRatings" when the user toggles a UNSAT rating (it stages { ratings: ... }). If setSection REPLACES the section object rather than merging, then visiting the CRM Ratings tab and touching a rating would WIPE the pending `findings` array — reproducing this exact bug even after the isEditing fix.

Show me the implementation of setSection in FormChangesContext and confirm merge vs replace. If it replaces, tell me — do not silently work around it.

Apply the rowsToRender fix and show the diff. STOP after applying.

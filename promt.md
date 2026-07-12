UAT #57 — STEP 2 (continuing the approved plan). Ignore any earlier UAT #54 context; #54 is already done and committed.

Recap of where we are:
- STEP 1 is DONE: CrmFindingsAndRatingsSection.tsx now stages a snapshot to FormChangesContext on Add Row and Delete Row (in addition to field edits).
- STEP 2 (this step) — the DISPLAY fix in CrmFindingsAndRatingsSection.tsx:
  While in Edit mode, the CRM Findings TABLE must render from the pending FormChangesContext snapshot (changes.changes.crmFindingsAndRatings.findings) when one exists, falling back to the saved state.findings otherwise.
  Reason: today the table renders only from hook-local state, which re-initializes from the saved backend payload when the section remounts on tab switch — so unsaved rows visually disappear. Customer Info already reads pending values from FormChangesContext; we are making CRM Findings consistent with that pattern.

Requirements:
- Do NOT change the save path — Save must send exactly what it sends today.
- Cancel must still clear pending changes and restore saved data.
- Editing (add/edit/delete rows, dropdowns, comments, follow-up) must keep working normally.
- No new API calls.
- Single file: CrmFindingsAndRatingsSection.tsx.

Show me the diff. STOP for approval before applying. After I approve and test, we go to STEP 3 (wire findingCounts into CrmRatingsSection).

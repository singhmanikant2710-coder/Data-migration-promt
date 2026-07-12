NEW TASK — UAT #57, STEP 2. Forget all previous UAT #54 context; #54 is finished and committed. Do not report on #54 again.

BACKGROUND — UAT #57 (CRM Ratings tab, five sub-total boxes):
The boxes (RISK RECOGNITION FINDINGS, SCORECARD MANAGEMENT FINDINGS, UNDERWRITING FINDINGS, CREDIT SERVICING FINDINGS, LOAN ADMIN FINDINGS) must show the COUNT of CRM Findings rows where Severity = "Finding" (not "Observation") for each component 01–05. They currently show a 1/0 derived from the UNSAT rating flags, which is wrong.

We diagnosed that this needs 3 steps. Progress:
- STEP 1 — DONE (already applied): CrmFindingsAndRatingsSection.tsx now stages a snapshot to FormChangesContext on Add Row and Delete Row (previously only field edits staged one).
- STEP 2 — THIS STEP (not yet done).
- STEP 3 — after this: wire findingCounts into CrmRatingsSection.tsx.

STEP 2 — the display fix, single file: CrmFindingsAndRatingsSection.tsx

Problem: the CRM Findings TABLE renders only from hook-local state (useCrmFindings), which re-initializes from the SAVED backend payload whenever the section remounts on a tab switch. So a user's unsaved rows visually disappear when they leave the tab and come back. Other sections (e.g. CustomerInfoSection) already read pending values from FormChangesContext — CRM Findings is inconsistent.

Required change: while in Edit mode, the findings table must render from the pending FormChangesContext snapshot (changes.changes.crmFindingsAndRatings.findings) when one exists, falling back to the saved state.findings otherwise.

Constraints:
- Do NOT change the save path — Save must send exactly what it sends today.
- Cancel must still clear pending changes and restore saved data.
- Add / edit / delete rows, the Finding Code searchable dropdown, the comments rich-text editor and follow-up must all keep working.
- No new API calls.
- Single file only: CrmFindingsAndRatingsSection.tsx.

Show me the diff. STOP for approval before applying.

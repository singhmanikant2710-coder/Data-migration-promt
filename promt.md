UAT #52 — CRM Findings: "Finding Description" must update based on the selected/clicked finding row.

I'm attaching two screenshots:
1. CLIENT EXPECTED (Access prototype): the Finding Description field at the top shows the description of the finding row the user has clicked/highlighted. Clicking a different row updates it.
2. CURRENT APP: the Finding Description box does not change when the user clicks different finding rows.

CONTEXT (already in place from UAT #53 — reuse, do not refetch):
- The CAS Findings library (code → description) is already fetched in this screen for the Finding Code dropdown labels. Every row's finding code already has its description available client-side.
- The dropdown now renders "CODE - Description" and the option value remains the raw finding code.

YOUR TASK:
1. FIRST investigate and REPORT (read-only, no edits):
   a. Where the Finding Description box currently gets its value (exact JSX + state/field it reads).
   b. Whether that field is a SAVED/persisted DB field or a purely display field. This is critical — if it is persisted, we must NOT overwrite saved data when re-rendering it.
   c. Whether the component tracks a "selected/active row"; if not, the minimal way to add it.
2. THEN propose ONE minimal fix so that clicking/focusing a finding row updates the Finding Description box with that row's finding code description (sourced from the already-fetched library — no new fetch).

Requirements:
- No new API calls / no fetch loops.
- Do NOT change the save path or overwrite persisted data.
- Row click/focus should visually indicate the active row (subtle highlight), consistent with existing app styling.
- Must work for all components (CS-*, SS-*, etc.).

Report your findings and proposed plan with the exact files it touches. STOP and wait for my approval before editing.

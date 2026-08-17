READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

STEP 1 — Locate files (commands, show paths ONLY, do not open):
grep -rl "Scorecard Results\|ScorecardResults" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs
grep -rl "scorecard-results" frontend/src backend/src --include=*.ts --include=*.tsx --include=*.cs

Show only paths. Then identify: which file is the Scorecard Results PDF component, and which builds its data (backend repo/service or frontend).

STEP 2 — Once paths known, read the Scorecard Results PDF component ONCE + reference (CrmSummaryPDF.tsx, CrmPdGradeMigrationPDF.tsx) for pattern. Findings only, no edits:

=== ITEM 1 — Header font ===
1. The header title style + current fontSize in the Scorecard Results PDF.

=== ITEM 2 — Right header: sample name -> run date ===
2. What the right-hand header currently shows (sample name / caption). Does this component receive a run date / generatedOn prop? Show the props/type. (If not available, flag it.)
3. How CrmSummaryPDF shows run date on the right (the pattern to match).

=== ITEM 3 — Count by accounts -> unique scorecard ID ===
4. Find where the summary counts/totals are computed (the "counting by accounts" logic). Show how records are counted — what field is counted (account vs scorecardId). Is this computed in the PDF component, or in the backend data (SQL/service)? Show the exact counting logic.

=== ITEM 4 — Scorecard Details tables: by Account -> unique scorecard ID ===
5. Find where the Scorecard Details table rows are built. Show how rows are derived (per-account vs per-unique-scorecardId). Show the data field used and any dedup/grouping logic (or lack of it).

=== ITEM 5 — Footer (remove FH logo, match CRM Summary/PD Migration) ===
6. The footer JSX + styles in Scorecard Results PDF. Show the First Horizon logo element specifically.
7. The footer pattern in CrmSummaryPDF / CrmPdGradeMigrationPDF (the "[Report Name] • Page X of Y" style to match).

CONSTRAINTS:
- Read each file ONCE. Findings only. No edits.
- For items 3 & 4: clearly state whether the count/detail logic lives in the PDF (frontend) or in backend data (SQL query/service), since that determines whether this is a display change or a data-layer change.
- Flag any SHARED styles/components.

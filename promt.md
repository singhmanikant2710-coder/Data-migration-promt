READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/ScorecardResultsPDF.tsx

Geoff wants the Scorecard ID cell in the details table to show ONLY the Scorecard_id_bank value — currently it shows both bank and system IDs together when they differ.

Show me ONLY (no edits):
1. The current formatScorecardId function — full body. Confirm it returns `${bank} ${system}` (or bank\nsystem) when bank !== system.
2. Where formatScorecardId is called in buildGroups (the id assignment) and how the id is rendered in the Scorecard ID cell.
3. Confirm the field names: scorecardIdBank and scorecardIdSystem (the row properties it reads).
4. Is formatScorecardId used ANYWHERE else in this file (or others)? If it's only used for this details cell, changing it to bank-only is safe.

Read once. Findings only. No edits.

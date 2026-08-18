READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

Files (read each ONCE, findings only, no edits):
- backend/src/Casrr.Infrastructure/SqlServer/SqlScorecardResultsReportRepository.cs
- backend/src/Casrr.Application/Reporting/ScorecardResults/ScorecardResultsModels.cs
- backend/src/Casrr.Application/Reporting/ScorecardResults/ScorecardResultsReportService.cs
- frontend/src/components/pdf/ScorecardResultsPDF.tsx

I need to change both the summary count (Item 3) and the details table (Item 4) to work by UNIQUE Scorecard ID instead of by account. Before any edit, show me:

=== A. UNIQUENESS KEY (most important) ===
1. In the SQL repository: the query that pulls scorecard rows (the details source). Show ALL columns selected, especially any scorecard ID field(s). Is there a single unique scorecard ID column (e.g. Scorecard_id / ScorecardId), or is the ID composed of multiple fields (Bank ID + System ID)?
2. Show how the frontend formatScorecardId builds the displayed ID — which underlying field(s) it uses. This tells me what "unique scorecard ID" actually means in the data.

=== B. ITEM 3 — Summary count ===
3. The full ComputeTotalsAsync SQL (COUNT(*) ... GROUP BY Scorecard_assessment). Show it exactly.
4. Confirm it counts account rows (COUNT(*)) — and whether a COUNT(DISTINCT <scorecardIdColumn>) would be possible with the columns available in that table.

=== C. ITEM 4 — Details rows ===
5. In ScorecardResultsPDF.tsx buildGroups: show exactly how rows are pushed (per account row). Show what fields identify each row.
6. Confirm whether dedup should happen in the SQL/service (backend) or in buildGroups (frontend). Show enough of both to judge the safest place.

=== D. RECONCILIATION ===
7. Are the summary totals (backend) and the details rows (frontend) both derived from the SAME underlying rows? If we dedup one but not the other, will totals stop matching the row counts shown? Flag this.

CONSTRAINTS:
- Read each file ONCE. Findings only. No edits.
- I specifically need the exact unique-scorecard-ID column name, and whether dedup is safe in SQL vs frontend.
- Flag anything where deduping could drop or merge rows that differ (e.g. same scorecard ID but different assessment/values).

READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

File: frontend/src/components/pdf/CrmSummaryTablePDF.tsx
Reference files (read for pattern only, do NOT edit):
- frontend/src/components/pdf/CrmSummaryPDF.tsx
- frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Read each file ONCE (do not re-read, do not search wider codebase). Show me the following. Findings only — no edits, no suggested code.

=== ITEM 1 — Report header (name left + run date right) ===
1. In CrmSummaryTablePDF.tsx: the header/banner JSX + styles. Show the current LEFT text (report title) and what is shown on the RIGHT (currently "357 - 5/1/2026 - Examination - Wholesale").
2. In CrmSummaryPDF.tsx AND CrmPdGradeMigrationPDF.tsx: how their header renders the title on the LEFT and the run date on the RIGHT. Show the exact JSX + the variable/prop that holds the run date, so I can match the same pattern.

=== ITEM 2 — CRM Component sub-headers (light-red highlighted) ===
3. The JSX + style for the section sub-headers above each component table (e.g. "RISK RECOGNITION (4)", "SCORECARD MANAGEMENT (5)"). Show the style object and its current fontSize.
4. Is that sub-header style SHARED with any other heading in the file? List where it's used.

=== ITEM 3 — Footer ===
5. The footer JSX + style in CrmSummaryTablePDF.tsx. Show exactly what it currently renders (it appears to include a Sample Name + the "357 - 5/1/2026..." string + page numbers).
6. For reference: the footer JSX in CrmSummaryPDF.tsx and CrmPdGradeMigrationPDF.tsx — show the exact "[Report Name] - Page # of ##" pattern they use.
7. Identify the variable/prop holding the report name and the page-number render (fixed/render callback).

=== ITEM 4 — Duplicate filter payload on Page 4 ===
8. Search CrmSummaryTablePDF.tsx for "Current Filter Payload" AND "APPLIED REPORT FILTERS" AND "Applied Report Filters" — show EVERY occurrence with surrounding JSX.
9. There appear to be TWO filter-payload blocks. Show both, and what distinguishes them (which Page/branch each renders in — no-items branch vs normal final page, or a genuine duplicate on the last page).
10. Confirm which one is the correct "last page, following report details" block and which is the extra one to remove.

CONSTRAINTS:
- Read each file ONCE. Do not re-read. Do not open other files.
- Findings only. No edits.
- Flag any SHARED styles/components that items 1–4 might touch so nothing else breaks.

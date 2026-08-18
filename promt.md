READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Read ONCE. Findings only, no edits. Show me the following.

=== ITEM 1 — Font 11 for matrices + table headers (light red highlighted) ===
1. Identify the matrix tables and their table-header styles (the "light red highlighted" headers). Show the style object(s) controlling their fontSize and current value.
2. Flag if these header styles are SHARED across multiple tables/matrices.

=== ITEM 2 — Font 9 for blue column headers + values ===
3. Identify the "blue column headers" and their VALUES in the matrices. Show the style object(s) controlling fontSize for both the blue headers and the value cells. Current values.
4. Flag sharing — is the blue-header style and the value style used in multiple places?

=== ITEM 3 — Vertical centering ===
5. In the Accounts matrix: find the "CAS PD Totals" row. Show its cell styles — specifically alignItems / justifyContent / textAlign / any vertical alignment. Why might it not be vertically centered?
6. In the Commitment matrix: find the value cells and totals. Show their vertical-alignment styles. 
7. Show the base cell style these use, and flag if shared.

=== ITEM 4 — Rename + reposition filter payload ===
8. Search for "Current Filter Payload" / "APPLIED REPORT FILTERS" / "Applied Report Filters" — show every occurrence with surrounding JSX and which Page/branch.
9. Show the Document structure: which Page holds the details table, and the separate Page holding the filter payload. I need to see if I can move the filter block into the details Page's flow (like we did for CrmFindingsObservations), and whether the details Page auto-flows. Show the details-table component/Page and where the filter Page sits.
10. Does the details Page / component receive `filters`? Show its props.

=== SHARED CHECK ===
11. List

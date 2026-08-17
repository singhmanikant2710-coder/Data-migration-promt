
READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

Primary file: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx
Reference (read for pattern only, do NOT edit): frontend/src/components/pdf/CrmSummaryPDF.tsx

Read each file ONCE (no re-read, no wider search). Findings only, no edits, no suggested code.

=== ITEM 1 — Header (font size, width, page-1-only) ===
1. In CrmFindingsObservationsPDF.tsx: the header/banner JSX + styles (headerBar/headerTitle/headerMeta equivalents). Show current fontSize, and how the header container width is set (does it span full page width? any margin/padding/width style?).
2. Is the header rendered ONCE (page 1 only) or does it repeat on every page? Look for `fixed` prop on the header View, or the header being inside a per-page render. Show exactly how/where it's placed.
3. In CrmSummaryPDF.tsx (reference): the header fontSize, the header container width/margins, and confirm it renders on page 1 only (not fixed/repeating). Show the exact style + placement so I can match it.

=== ITEM 2 — Severity / Comments column widths ===
4. The findings/observations table: show the column definitions/width styles for SEVERITY and COMMENTS columns (and any shared column-width style). Are these fixed % / flex? Show all column widths in that table so the total is clear.
5. Flag if the column-width style is SHARED across other tables/sections in this file.

=== ITEM 3 — Table header vs row vertical gridlines alignment ===
6. Show the table header cell styles vs the data row cell styles — specifically borderRight / borderLeft / padding / width. I need to see why header vertical gridlines might not line up with row gridlines (mismatched border or padding between header cells and body cells).

=== ITEM 4 — Table column header font size (-> 9) ===
7. The style controlling the table COLUMN HEADER text size (e.g. thText equivalent). Show its current fontSize and whether it's shared with anything else.

=== ITEM 5 — "Current Filter Payload" rename + reposition ===
8. Search this file for "Current Filter Payload" / "APPLIED REPORT FILTERS" / "Applied Report Filters" — show every occurrence with surrounding JSX and which Page/branch it's in.
9. Show the structure: where do the detail tables end, and where does the filter payload page sit (separate <Page>? last page?). I need to understand current placement vs "directly below detail tables".

CONSTRAINTS:
- Read each file ONCE. Findings only. No edits.
- Flag any SHARED styles/components that items 1-5 might touch so nothing else breaks.

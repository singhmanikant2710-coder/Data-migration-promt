READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

Read each of these files ONCE (do not re-read, do not search the wider codebase):
- frontend/src/components/pdf/CrmSummaryPDF.tsx        (main report — #182)
- frontend/src/components/pdf/CrmSummaryTablePDF.tsx   (table report — #181)
- frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx (reference for header fontSize=14 pattern only)

Show me the following. Findings only — no edits, no suggested code.

1. HEADER FONT SIZE (item 1):
   - In CrmSummaryPDF.tsx: the style rendering the "CRM Summary" report title/header text, and its exact current fontSize.
   - In CrmPdGradeMigrationPDF.tsx: how its equivalent report header sets fontSize=14 (the exact style block).
   - Is the CRM Summary header style local to this file, or imported/shared? Show the style definition and where it's applied.

2. SCORECARD ASSESSMENT TABLE — Bank PD / Bank LGD / CAS PD / CAS LGD headers (item 2):
   - The JSX + style for these 4 column-header cells.
   - Their current textAlign value.
   - Is the header-cell style SHARED with the other columns in that same table (Scorecard ID, Date, Scorecard Type, Scorecard Assessment)? Show me the exact style object(s) so I can tell whether centering only these 4 is possible without touching the others.

3. SCORECARD ID cell overflow (item 3):
   - The cell/View rendering the Scorecard ID VALUE (not header).
   - Its exact width / flex / maxWidth / overflow / wrap props.
   - Is this cell's style shared with other value cells in the row? Show the style object.
   - Note whether the row uses fixed widths or flex, since that affects how wrapping will behave.

4. "Current Filter Payload" heading (item 4):
   - Which file contains the string "Current Filter Payload" (grep both CRM files if unsure — show path).
   - The exact JSX/line where it's rendered.
   - Confirm it is a plain display heading only, NOT a key used in data, filtering, or any logic.

CONSTRAINTS:
- Read each file ONCE. Do not re-read. Do not open other files.
- Findings only. No edits.
- Explicitly flag any SHARED style/component that items 2 and 3 might touch, since centering the 4 headers and adding wrap to Scorecard ID are the two risky ones for breaking other columns.

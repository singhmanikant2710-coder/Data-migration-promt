READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Three remaining LAYOUT-ONLY issues in the matrices (MatrixCount + 
MatrixCommitment). Do not touch calculations, colors, or the page break.

ISSUE A (header font smaller than row data): The matrix header text (BANK PD, 
1-16, 13/SM, 14/SUB etc.) appears SMALLER than the row data values below it, 
making the hierarchy look inverted. Show:
1. The header cell fontSize — styles.thCompact (I believe fontSize 7) applied 
   to header cells.
2. The DATA row cell fontSize — styles.td (what fontSize? likely inherits 
   page fontSize 9, or has its own).
3. So header (7) < data (9?) — confirm the exact numbers. Client wants header 
   >= data size (ideally 1pt larger), keeping bold + centered, WITHOUT 
   increasing row height much. Show both fontSizes so I can raise the header 
   to match or exceed data.

ISSUE B (table border overflow beyond printable width): The matrix table 
border extends slightly beyond page margins (left/right), bottom border wider 
than content. Investigate:
1. styles.table — width, borders, padding, margin.
2. styles.tr and cell styles (styles.thDark, styles.td) — borderRightWidth 
   on each cell (per-cell borders add pixels BEYOND the flexBasis %).
3. Column widths sum: BANK PD 8% + 16×4% + Totals 12% + # Changes 8% + 
   % Change 8% = 100%. With ~20 cells each having borderRightWidth: 1, that's 
   ~20px added beyond 100% width → overflow. Confirm this is the cause.
4. styles.page paddingLeft/Right (MARGINS) — the printable content width.
5. Is there box-sizing handling, or does 100% flexBasis + per-cell borders 
   exceed the content width?

ISSUE C (Totals+footnote whitespace on page 2): The Totals row + footnote 
isolate to page 2 leaving blank on page 1. Show:
1. Current structure: data rows -> Totals (wrap={false}) -> footnote (separate 
   sibling, from earlier fix).
2. Is the Totals row genuinely not fitting on page 1 (matrix too tall), or is 
   spacing/margin pushing it? Show styles.table marginBottom, any spacer, 
   styles.trLast.
3. The matrix height vs page height for a large sample.

Do not edit anything. Show: header vs data fontSize (Issue A), table/cell 
width + border math (Issue B), Totals/footnote structure + heights (Issue C). 
Findings only.

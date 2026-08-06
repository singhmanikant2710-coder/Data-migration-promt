Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix value overlap in MatrixCommitment ONLY. The 16 CAS PD columns are 4.25% 
wide, but commitment values with decimals (e.g. "$189.1", "$1,234.56") are too 
wide for 4.25% at fontSize 10, causing overlap/crowding. Number of Accounts 
(integers) doesn't have this problem — leave MatrixCount unchanged.

Fix: reduce the fontSize of ONLY the CAS PD column cells in MatrixCommitment 
(both the data rows and the bottom Totals row) from 10 to 8, so decimal 
currency values fit within 4.25% without overlapping. Do not change widths.

In MatrixCommitment ONLY:
1. Data row CAS PD cells (inside r.cells.map): add fontSize: 8 to the cell 
   style (currently inherits styles.td fontSize 10):
     style={[styles.td, { flexBasis: "4.25%", ..., fontSize: 8, ... }]}
2. Bottom Totals row CAS PD cells (the colTotals.map cells): add fontSize: 8 
   the same way.

Do NOT change:
- MatrixCount (integers fit fine — leave fontSize 10).
- The BANK PD, Bank PD Totals, # Changes, % Change cells (they have wider 
  columns 8%, values fit) — leave them at 10.
- Widths, colors, alignment, fmt(), or the label.

Only reduce fontSize to 8 on the 16 CAS PD cells (data + Totals row) in 
MatrixCommitment, so decimal values fit without overlap.

CONSTRAINTS:
- MatrixCommitment ONLY; MatrixCount unchanged.
- Only the 16 CAS PD column cells (data rows + bottom Totals row) get fontSize 
  8; other columns stay 10.
- Do NOT change widths, colors, alignment, or fmt().
- Only edit this one file. Show the updated CAS PD data cell and Totals cell 
  in MatrixCommitment.

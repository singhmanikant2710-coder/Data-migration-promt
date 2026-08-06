Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Increase the font size of the Commitment matrix values to match the row totals 
(fontSize 10). MatrixCommitment ONLY. Client (Geoff) wants the CAS PD 
commitment values and column totals to match the other cells' font size.

Currently the CAS PD cells (data rows + bottom Totals row) in MatrixCommitment 
use fontSize: 8 (we reduced it earlier to prevent overlap). Now that values 
are shorter (no "$" prefix, 1 decimal), raise them to fontSize 10 to match the 
row totals and other columns.

In MatrixCommitment ONLY:
1. CAS PD data cells (inside r.cells.map): change fontSize: 8 to fontSize: 10 
   (or remove the fontSize override so it inherits styles.td's 10).
2. Bottom Totals row CAS PD cells (colTotals.map): change fontSize: 8 to 
   fontSize: 10.

Keep everything else (widths 4.25%, the wrap/overflow handling from the 
overlap fix, colors, alignment, fmt) unchanged.

CONSTRAINTS:
- MatrixCommitment ONLY. MatrixCount unchanged.
- Only change fontSize 8 -> 10 on the CAS PD data cells and Totals row CAS PD 
  cells.
- Keep the existing overflow/wrap/whiteSpace handling and widths (so if a rare 
  large value doesn't fit, it clips cleanly rather than overflowing).
- Do NOT change fmt(), calculations, colors, labels, or the header.
- Only edit this one file. Show the updated CAS PD data cell and Totals cell 
  with fontSize 10.

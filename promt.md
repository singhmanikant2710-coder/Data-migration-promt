Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Center-align all cell values in BOTH matrices. Client wants every matrix cell 
value centered. Change textAlign to "center" — do NOT change widths, colors, 
text, or logic.

In BOTH MatrixCount and MatrixCommitment, change textAlign to "center" on 
these cells that are currently "right":

DATA ROWS:
- The row-sum (Totals) cell: textAlign "right" → "center"
- The # Changes cell: textAlign "right" → "center"
- The % Change cell: textAlign "right" → "center"
- (MatrixCommitment only) the 16 CAS PD cells: currently "right" → "center"
  (In MatrixCount the CAS PD cells are already "center" — leave them.)
- BANK PD first cell: already center (leave as-is).

BOTTOM TOTALS ROW:
- The 16 colTotal cells: ensure "center" (MatrixCount already center; 
  MatrixCommitment currently "right" → "center")
- grandTotal cell: "right" → "center"
- grandRowChanges (# Changes total) cell: "right" → "center"
- % Change total cell: "right" → "center"

HEADER ROW (labels):
- "Bank PD Totals" header: ensure "center"
- "# Changes" header: already center (leave)
- "% Change" header: already center (leave)
- The 16 CAS PD headers: already center (leave)

CONSTRAINTS:
- ONLY change textAlign values to "center" on the cells listed above, both 
  matrices.
- Do NOT change widths, colors, fmt, data, text, or the footnote.
- Do NOT touch pageSetup.
- Only edit this one file. Show the centered cells (data rows, Totals row, 
  header) in both matrices.

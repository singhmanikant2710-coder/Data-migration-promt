Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Three FINAL layout fixes for BOTH MatrixCount and MatrixCommitment. Do NOT 
change column widths, font sizes, colors, $0 blanking, or the page break.

=== FIX 1: Table border overflow (keep widths 100%, fix border rendering) ===
The per-cell borderRightWidth: 1 on ~20 cells adds ~20pt beyond the 100% 
flexBasis, overflowing the printable width. Fix WITHOUT changing widths:

a) On styles.table, ensure it clips overflow and stays within bounds. Add/
   confirm:
     overflow: "hidden",
     width: "100%",
   (This clips any border overflow to the table's box.)

b) Remove the right border from the LAST cell in EVERY row (it's redundant — 
   the table's outer border already closes the right edge). In BOTH matrices, 
   add borderRightWidth: 0 (or apply styles.tdLast) to the LAST cell (% Change 
   column) of:
   - Header row 1 (grouping row - last cell)
   - Header row 2 (labels - the % Change header cell)
   - Every data row (the % Change data cell)
   - The Totals row (last cell)
   So the last column has NO right border, reducing cumulative width and 
   letting the table fit within 720pt.

=== FIX 2: Reduce header height (keep font 10, cut padding/lineHeight) ===
The header font is 10 (keep it — readable). Reduce ONLY padding and lineHeight 
in styles.thCompact to shrink header height so Totals fits on page 1:
   Change styles.thCompact from:
     { fontSize: 10, padding: 3, lineHeight: 1.1 }
   to:
     { fontSize: 10, padding: 2, lineHeight: 1.0 }
(Font stays 10, but tighter padding/lineHeight reduces header row height, 
giving Totals a better chance to stay on page 1.)

=== FIX 3: Footnote width matches table ===
Ensure the footnote container width matches the table exactly (full table 
width, no overflow). On the footnote <Text>, add width: "100%" so it aligns 
with the table's width, and confirm it's a direct sibling of the table within 
the same matrix root View (so it aligns to the same left/right edges).
   Footnote style becomes:
     { fontSize: 8, color: "#475569", marginTop: 4, marginBottom: 10, fontStyle: "italic", width: "100%" }

CONSTRAINTS:
- Do NOT change column widths (keep 8% + 16×4% + 12% + 8% + 8% = 100%).
- Do NOT change font sizes (header stays 10, data stays 10).
- Do NOT change colors, $0 blanking, zero-rounding, row shading, or the page 
  break between matrices.
- FIX 1: overflow:hidden + width:100% on styles.table, and borderRightWidth: 0 
  on the last cell of every row (both matrices).
- FIX 2: thCompact padding 3->2, lineHeight 1.1->1.0 (font stays 10).
- FIX 3: footnote width: "100%".
- Apply to BOTH matrices identically.
- Do NOT touch pageSetup.ts, page size, margins, or backend.
- Only edit this one file. Show the styles.table change, the last-cell border 
  removal, the thCompact change, and the footnote width — for both matrices.

Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Three LAYOUT-ONLY fixes for BOTH MatrixCount and MatrixCommitment. Do NOT 
change calculations, colors, data, the new columns, subreport shading, or the 
page break.

=== ISSUE A: Header font too small (7) vs data (10) ===
The matrix header cells use styles.thCompact (fontSize 7) while data cells use 
styles.td (fontSize 10), inverting the hierarchy. Raise the header font to 
match/exceed the data.

Change styles.thCompact fontSize from 7 to 10 (matching data), keeping it 
compact otherwise. To avoid increasing row height too much, keep padding 
small:
    thCompact: { fontSize: 10, padding: 3, lineHeight: 1.1 }
(fontSize 10 = same as data; bold comes from styles.thDark fontWeight 700; 
centering is already applied per earlier fix. Padding 3 + lineHeight 1.1 
keeps the header row reasonably tight.)

This applies to both matrices' headers (they share thCompact). Verify the 
13/SM, 14/SUB labels still fit on the columns at fontSize 10 — if they wrap, 
that's acceptable (2 lines) since the columns are 4% wide; the header is now 
prominent.

=== ISSUE B: Table border overflow beyond printable width ===
The row has ~20 cells, each with borderRightWidth: 1, adding ~20pt BEYOND the 
100% flexBasis width (react-pdf draws borders in addition to the content box). 
So the table (100% + 20pt) exceeds the 720pt printable width, overflowing 
left/right.

Fix: remove the right border from the LAST cell in each row so the cumulative 
border width is reduced, AND ensure the outer table border contains it. Apply 
styles.tdLast (borderRight: 0) to the LAST column cell (% Change) in:
- Both header rows (the last header cell)
- All data rows (the last data cell)
- The Totals row (last cell)
in BOTH matrices.

Additionally, if overflow persists, reduce the total column width slightly to 
leave room for the internal borders: change the widths so they sum to ~99% 
instead of 100% (e.g. reduce the 16 PD columns from 4% to 3.9%, giving 16×3.9 
= 62.4%, total 8 + 62.4 + 12 + 8 + 8 = 98.4%), leaving ~1.6% (~11pt) for the 
per-cell borders so the table fits within 720pt. Keep header and body widths 
mirrored.

Prefer the tdLast approach first (removing last-column right border); only 
reduce widths if the table still overflows.

=== ISSUE C: Totals row isolating to page 2 with whitespace ===
The Totals row is in <View wrap={false}>. On tall matrices, it can't fit at 
page 1 bottom so the whole block moves to page 2, leaving blank space.

Fix: REMOVE wrap={false} from the Totals row. A single Totals row is one line 
— it won't split across pages anyway (single-line row), and without wrap=false 
it will flow naturally into the remaining space on page 1 instead of jumping 
to page 2. The footnote (already a separate sibling) flows after it.

Change:
    <View wrap={false}>
      <View style={[styles.tr, styles.trLast]}> ...Totals... </View>
    </View>
to just:
    <View style={[styles.tr, styles.trLast]}> ...Totals... </View>
(remove the wrap={false} wrapper). Apply to both matrices.

This lets the Totals row + footnote fill page 1's remaining space, minimizing 
whitespace, while the page break before the Commitment matrix stays intact.

CONSTRAINTS:
- Issue A: only change thCompact fontSize 7 -> 10 (keep padding 3, lineHeight 
  1.1). Applies to both matrices' headers.
- Issue B: add tdLast (borderRight: 0) to the last column in header/body/
  Totals rows, both matrices. Only reduce widths if overflow persists.
- Issue C: remove the wrap={false} wrapper around the Totals row in both 
  matrices.
- Do NOT change data, calculations, colors, the new columns, subreport 
  shading, or the page break between matrices.
- Do NOT touch pageSetup.ts, page size, margins, or backend.
- Only edit this one file. Show: thCompact change, tdLast on last columns, 
  and the Totals row wrap removal for both matrices.

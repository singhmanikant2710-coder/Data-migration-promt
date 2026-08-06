Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Two layout refinements for BOTH MatrixCount and MatrixCommitment.

=== ISSUE 1: Keep Totals + Changes + % Change together (no split across pages) ===
Currently the Totals row, Changes row, and % Change row are three separate 
sibling <View> rows with no grouping, so they can split across a page 
boundary. Wrap these THREE summary rows in a single <View wrap={false}> parent 
in BOTH matrices, so they always stay together — if they don't fit at the 
bottom of a page, all three move to the next page as one unit (never split).

In BOTH MatrixCount and MatrixCommitment, wrap the three summary row <View>s:
    <View wrap={false}>
      {/* Totals row */}
      <View style={[styles.tr]}> ...existing... </View>
      {/* Changes row */}
      <View style={[styles.tr]}> ...existing... </View>
      {/* % Change row */}
      <View style={[styles.tr, styles.trLast]}> ...existing... </View>
    </View>

Keep the DATA rows above this group exactly as-is (ungrouped, no wrap — they 
flow/fill the page normally). Only the three summary rows get grouped.

=== ISSUE 2: Improve note spacing (note cannot go below the fixed footer) ===
React-PDF's footer is fixed/absolute at the page bottom, so the note CANNOT be 
placed below it. Instead, improve spacing so the note isn't crowded against 
the table and isn't cramped against the next section.

For the note in BOTH matrices, currently:
    <Text style={{ fontSize: 7, color: "#475569", marginTop: 4, fontStyle: "italic" }}>
Change marginTop from 4 to a larger gap and add marginBottom for separation 
from the next section:
    <Text style={{ fontSize: 7, color: "#475569", marginTop: 10, marginBottom: 12, fontStyle: "italic" }}>

This gives:
- ~18pt gap between the table (marginBottom 8) and the note (marginTop 10) — 
  clear separation from the Totals block.
- ~12pt gap below the note before the next section (MatrixCommitment / 
  DistCharts) — the note won't be cramped against the next table.

Apply the same marginTop: 10, marginBottom: 12 to the note in BOTH matrices.

CONSTRAINTS:
- Issue 1: wrap ONLY the three summary rows (Totals/Changes/%Change) in a 
  <View wrap={false}> in both matrices. Data rows stay ungrouped.
- Issue 2: only change the note's marginTop (4->10) and add marginBottom: 12 
  in both matrices. Do NOT attempt to place the note below the fixed footer 
  (not supported by React-PDF).
- Do NOT change data, calculations, colors, labels, column widths, the header, 
  the page footer, or the section order.
- Do NOT touch pageSetup.ts, page size, margins, or backend.
- Only edit this one file. Show the grouped summary rows and the updated note 
  styling for both matrices.

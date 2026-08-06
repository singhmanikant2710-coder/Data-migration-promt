Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Two remaining fixes.

=== ISSUE 1: Blank colored cells that DISPLAY as "$0" (rounding-aware) ===
In MatrixCommitment, the blank condition (bg && (!v || v === 0)) misses tiny 
non-zero values (e.g. 0.4 $MM = $400k) that fmt() rounds to "$0" (fmt uses 
maximumFractionDigits: 0, so anything < $0.5MM shows "$0"). So colored cells 
can still show "$0".

Fix: change the commitment cell blank condition to align with the formatter — 
blank when the cell is colored AND fmt(v) would display "$0" (i.e. rounds to 
zero in $MM). Change:
    {(bg && (!v || v === 0)) ? "" : fmt(v)}
to:
    {(bg && Math.round(v) === 0) ? "" : fmt(v)}

(Math.round(v) === 0 matches fmt()'s whole-$MM rounding — so any colored cell 
that would show "$0" is blanked instead. White/diagonal cells still show 
fmt(v) as before.)

Do NOT change MatrixCount's blank logic (counts are integers, v === 0 is 
already correct there — a count of 0 is exactly 0, no rounding issue).

=== ISSUE 2: Reduce Totals+footnote isolation on page 1 ===
Currently the Totals row AND footnote are together in one <View wrap={false}>. 
When the matrix is tall, this whole block moves to the next page, leaving a 
big blank on page 1.

Fix: SEPARATE the footnote from the wrap group. Keep ONLY the Totals row in 
<View wrap={false}> (so the Totals row itself never splits), and move the 
footnote OUTSIDE that group as an independent sibling that can flow naturally:

    {/* Totals row - kept intact, but small (1 row) so it fits more easily */}
    <View wrap={false}>
      <View style={[styles.tr, styles.trLast]}> ...Totals... </View>
    </View>
    {/* Footnote - independent, flows after Totals; not bound so it won't 
        force the Totals group to jump pages */}
    <Text style={{ fontSize: 8, color: "#475569", marginTop: 4, marginBottom: 10, fontStyle: "italic" }}>
      CAS PD Rating totals represent... (unchanged text)
    </Text>

This makes the wrap group just the single Totals row (small footprint, fits 
on page 1 more often). The footnote flows after it — if the footnote spills 
to the next page, that's a minor text move, not a big blank block.

Apply BOTH fixes to the respective matrices (Issue 1: MatrixCommitment only; 
Issue 2: both MatrixCount and MatrixCommitment).

CONSTRAINTS:
- Issue 1: only change MatrixCommitment's cell blank condition to 
  Math.round(v) === 0. Keep MatrixCount as-is.
- Issue 2: keep ONLY the Totals row in <View wrap={false}>; move the footnote 
  out as an independent sibling. Apply to both matrices.
- Keep the page break between the two matrices (unchanged).
- Do NOT change data, calculations, colors, the new columns, or subreport 
  shading.
- Do NOT touch pageSetup.ts, page size, margins, or backend.
- Only edit this one file. Show the commitment cell condition and the 
  separated Totals/footnote structure for both matrices.

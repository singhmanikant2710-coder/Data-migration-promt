Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix (Item 3, MatrixCommitment body rows): The diagonal value cells (tdClamp: padding 2, fontSize 9) and the right-side Bank PD Totals / # Changes / % Change cells (td: padding 6, fontSize 10) sit at different vertical positions within the same row because of mismatched padding and font size. Align them by giving the three right-side cells the same padding (2) and fontSize (9) as the value cells. Do NOT use alignItems (it caused line artifacts) and do NOT add tdClamp to these cells (they hold large numbers that must not clip).

In the MatrixCommitment BODY rows, update the three right-side cells:

BEFORE (sum cell):
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, textAlign: "center" }]}>{r.sum === 0 ? "0.0" : fmt(r.sum)}</Text>
AFTER:
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, textAlign: "center", fontSize: 9, padding: 2 }]}>{r.sum === 0 ? "0.0" : fmt(r.sum)}</Text>

BEFORE (rowChanges cell):
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, textAlign: "center" }]}>{r.rowChanges === 0 ? "0.0" : fmt(r.rowChanges)}</Text>
AFTER:
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, textAlign: "center", fontSize: 9, padding: 2 }]}>{r.rowChanges === 0 ? "0.0" : fmt(r.rowChanges)}</Text>

BEFORE (rowPct cell):
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, textAlign: "center" }, styles.tdLast]}>{`${r.rowPct.toFixed(1)}%`}</Text>
AFTER:
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, textAlign: "center", fontSize: 9, padding: 2 }, styles.tdLast]}>{`${r.rowPct.toFixed(1)}%`}</Text>

Also update the fromPd LABEL cell (leftmost) to match so the whole row aligns:
BEFORE:
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0 }, styles.tdCenter]}>{String(r.fromPd)}</Text>
AFTER:
<Text style={[styles.td, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, fontSize: 9, padding: 2 }, styles.tdCenter]}>{String(r.fromPd)}</Text>

CONSTRAINTS:
- ONLY change the MatrixCommitment BODY row cells (label + sum + rowChanges + rowPct). Add fontSize: 9 and padding: 2 to match the value cells.
- Do NOT add tdClamp/overflow:hidden to these cells (large numbers must not clip).
- Do NOT use alignItems.
- Do NOT change the diagonal value cells (already fontSize 9, padding 2 via tdClamp).
- Do NOT touch MatrixCount, Detail, Subreports, or shared styles.
- Show the FULL diff.

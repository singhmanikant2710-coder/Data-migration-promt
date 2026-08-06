Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix number-breaking and overflow in MatrixCommitment CAS PD cells — a GENERIC 
rendering fix that works for ALL datasets/values (no value-specific logic). 
MatrixCommitment ONLY; MatrixCount unchanged.

Problem: styles.td has wordBreak: "break-word", so currency values break 
mid-number (e.g. "$1,234" then ".56"). Values must stay on one line, fit 
inside the 4.25% cell, and never spill into neighbors — for every dataset.

Apply to BOTH the CAS PD data cells (r.cells.map) AND the bottom Totals row 
CAS PD cells (colTotals.map) in MatrixCommitment. Add to each cell's inline 
style:
    wordBreak: "keep-all",
    whiteSpace: "nowrap",
    overflow: "hidden",
    padding: 2,

So a CAS PD data cell becomes:
    style={[styles.td, {
      flexBasis: "4.25%", flexGrow: 0, flexShrink: 0,
      textAlign: "center", fontSize: 8,
      wordBreak: "keep-all", whiteSpace: "nowrap", overflow: "hidden", padding: 2,
      backgroundColor: bg, color: fg,
    }]}

And a Totals row CAS PD cell becomes:
    style={[styles.td, {
      flexBasis: "4.25%", flexGrow: 0, flexShrink: 0,
      textAlign: "center", fontSize: 8,
      wordBreak: "keep-all", whiteSpace: "nowrap", overflow: "hidden", padding: 2,
    }]}

This is uniform for all cells (no per-value conditions), so it behaves 
identically across all sample datasets.

CONSTRAINTS:
- MatrixCommitment ONLY (CAS PD data cells + Totals row CAS PD cells). 
  MatrixCount unchanged.
- Do NOT change fmt(), calculations, widths (4.25% stays), colors, labels, or 
  other columns.
- Keep fontSize 8 (no further reduction).
- Uniform rendering rules — no value-specific or dataset-specific logic.
- Only edit this one file. Show the updated CAS PD data cell and Totals cell.

Some Commitment column-total values may be large enough that they can't fully display inside the CAS PD cells at the current 4.25% width with decimal formatting. Values now stay on one line and clip cleanly rather than overflowing. If you'd like large totals fully visible, we'd either widen those columns slightly or show whole-dollar $MM (e.g. "$189" vs "$189.1") in the matrix. Small changes like the $40K are still captured. Let me know your preference.

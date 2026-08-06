Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Three changes to MatrixCommitment ONLY (the "PD Grade Migration by Commitment" 
matrix). Do NOT touch MatrixCount.

CHANGE 1 — Add "($MM)" to the Commitment matrix header:
The section title currently reads "PD Grade Migration by Commitment". Change it 
to include ($MM):
    "PD Grade Migration by Commitment ($MM)"
(This is the styles.sectionTitle text above the Commitment matrix table.)

CHANGE 2 + 3 — Reformat currency: numbers in millions, 1 decimal, NO "$" prefix:
The values are already in $MM (divided by 1,000,000). Change fmt() so it 
outputs a plain number with thousands separators and exactly 1 decimal place, 
WITHOUT the "$" sign. This also fixes spacing (removing "$" gives more room).

Replace the current fmt() (in MatrixCommitment) with:
    const fmt = (n: number) => {
      if (!isFinite(n)) return "0.0";
      try {
        return new Intl.NumberFormat("en-US", {
          minimumFractionDigits: 1,
          maximumFractionDigits: 1,
        }).format(n);
      } catch {
        return n.toFixed(1);
      }
    };
Examples: 1452.86 -> "1,452.9"; 189.08 -> "189.1"; 0.04 -> "0.0"; 22 -> "22.0".
(No "$" prefix, always 1 decimal, comma thousands separator.)

ALSO update the zero/special-case strings in MatrixCommitment that currently 
use "$0" — change them to "0.0" to match the new no-$ format:
- Data cell blanking stays: {bg && v === 0 ? "" : fmt(v)} (fmt now returns 
  "0.0" for zero, but colored zero cells are still blanked — keep that).
- Row sum: change {r.sum === 0 ? "$0" : fmt(r.sum)} to {r.sum === 0 ? "0.0" : fmt(r.sum)}
- Row changes: change {r.rowChanges === 0 ? "$0" : fmt(r.rowChanges)} to 
  {r.rowChanges === 0 ? "0.0" : fmt(r.rowChanges)}
- Totals row column cells: change {v === 0 ? "$0" : fmt(v)} to {v === 0 ? "0.0" : fmt(v)}
- Totals grand: change {grandTotal === 0 ? "$0" : fmt(grandTotal)} to 
  {grandTotal === 0 ? "0.0" : fmt(grandTotal)}
- Totals changes: change {grandRowChanges === 0 ? "$0" : fmt(grandRowChanges)} 
  to {grandRowChanges === 0 ? "0.0" : fmt(grandRowChanges)}

NOTE on spacing: with "$" removed and 1 decimal (not 2), values are shorter, 
so they fit better in the 4.25% cells. Keep the fontSize 8 on CAS PD cells 
(it still helps), and keep the existing wrap/overflow handling.

CONSTRAINTS:
- MatrixCommitment ONLY. MatrixCount unchanged (it shows counts, no currency).
- fmt() now returns plain numbers (no "$"), 1 decimal, comma separators.
- Replace all "$0" literals in MatrixCommitment with "0.0".
- Do NOT change calculations, widths, colors, labels, the footnote, or 
  pagination.
- Do NOT touch MatrixCount or subreports.
- Only edit this one file. Show the new header, the new fmt(), and the updated 
  zero-strings.

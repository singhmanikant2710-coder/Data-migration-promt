Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Two related changes for dollar formatting and un-suppressing small values.

=== CHANGE 1: fmt() shows decimals so small values stay visible ===
The current fmt() (in MatrixCommitment) uses maximumFractionDigits: 0, so a 
$40K migration ($0.04MM) rounds to "$0" and disappears. Client wants small 
values visible. Change fmt() to show decimals:

Current:
    const fmt = (n: number) => {
      if (!isFinite(n) || n === 0) return "$0";
      try {
        return new Intl.NumberFormat("en-US", {
          style: "currency", currency: "USD",
          maximumFractionDigits: 0,
        }).format(n);
      } catch { return `$${Math.round(n)}`; }
    };

Change to:
    const fmt = (n: number) => {
      if (!isFinite(n)) return "$0.0";
      try {
        return new Intl.NumberFormat("en-US", {
          style: "currency", currency: "USD",
          minimumFractionDigits: 1, maximumFractionDigits: 2,
        }).format(n);
      } catch { return `$${n.toFixed(1)}`; }
    };

(This shows $0.04 for a $40K migration and $22.0 for larger values — small 
values stay visible, large ones aren't cluttered. Removed the "n === 0 -> $0" 
early return so 0 shows as "$0.0" consistently.)

=== CHANGE 2: Stop blanking zero/small values in colored cells ===
Client now wants all values shown (reversing the earlier $0-blanking).

In MatrixCommitment data cells, change:
    {bg && Math.round(v) === 0 ? "" : fmt(v)}
to just:
    {fmt(v)}

In MatrixCount data cells, change:
    {bg && v === 0 ? "" : String(v)}
to just:
    {String(v)}

(Keep the bg/fg colors on the cells — only the blanking is removed. Now every 
cell shows its value, including small/zero ones.)

CONSTRAINTS:
- Change fmt() as shown (MatrixCommitment's formatter).
- Remove the colored-cell blanking in BOTH matrices (show all values).
- Keep the bg/fg color logic unchanged.
- Do NOT change widths, alignment, the footnote, or pageSetup.
- Only edit this one file. Show the new fmt() and the un-blanked cell renders 
  in both matrices.

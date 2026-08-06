Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Hide TRUE-ZERO values in the colored (green/red) data cells so changes pop, 
but keep SMALL non-zero values visible (e.g. a $40K change = $0.04MM must 
still show). Apply to BOTH matrices' data cells. Do NOT change the Totals row, 
widths, colors, or anything else.

MatrixCount colored data cells — currently: {String(v)} (shows "0" in colored 
cells). Change to hide true zero only in colored cells:
    {bg && v === 0 ? "" : String(v)}
(Colored cell + exactly 0 -> blank; colored cell + any nonzero -> show the 
number; white/diagonal cell -> show as-is including 0.)

MatrixCommitment colored data cells — currently: {fmt(v)} (shows "$0.0" in 
colored cells even for true zero). Change to hide true zero only in colored 
cells, but show small non-zero values:
    {bg && v === 0 ? "" : fmt(v)}
(Colored cell + exactly 0 -> blank; colored cell + tiny nonzero like 0.04 
($40K) -> fmt shows "$0.04"; white/diagonal cell -> show as-is.)

IMPORTANT: use "v === 0" (exact zero), NOT Math.round(v) === 0 — because a 
$40K value (0.04 in $MM) is NOT exactly 0, so it will correctly SHOW as 
"$0.04" and only TRUE zeros are blanked. This gives Geoff exactly what he 
asked: hide zeros, but pick up small changes above $0.

CONSTRAINTS:
- ONLY change the colored DATA cell value render in both matrices (the cell 
  inside r.cells.map).
- Keep the bg/fg colors unchanged (only the zero value is blanked).
- Do NOT change the bottom Totals row cells, the white/diagonal cells' 
  behavior, widths, or fmt().
- Only edit this one file. Show the updated colored-cell render in both matrices.

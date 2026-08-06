Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Three formatting changes (client-confirmed). Bounded — make only these three.

=== ITEM 5: Show "0" in MatrixCount Totals and Changes rows ===
In MatrixCount, the Totals row and Changes row currently render zero as NBSP 
(blank): {v === 0 ? NBSP : String(v)}. Change them to always show the number 
(including "0").

- Totals row column cells: change {v === 0 ? NBSP : String(v)} to {String(v)}
- Totals grand total: change {grandTotal === 0 ? NBSP : String(grandTotal)} 
  to {String(grandTotal)}
- Changes row column cells: change {v === 0 ? NBSP : String(v)} to {String(v)}
- Changes grand total: change the NBSP conditional to always String(...) 
  (show "0" when zero)

So every cell in the Totals and Changes rows of MatrixCount shows its numeric 
value, "0" included. (Body cells already show "0" — leave them as-is.)

=== ITEM 6: Show "$0" in MatrixCommitment body cells (keep colors) ===
In MatrixCommitment, the BODY cells currently render zero/empty as NBSP: 
{(v || v === 0) ? fmt(v) : NBSP} — wait, actually it shows NBSP when v is 0. 
Change the body cell so zero shows "$0" (fmt(0) already returns "$0"):

Change the body cell render from:
    {(v || v === 0) ? fmt(v) : NBSP}
to simply:
    {fmt(v)}
so zero commitment cells display "$0". 

IMPORTANT: Keep the existing directional colors on these body cells UNCHANGED 
(bg #fee2e2/#dcfce7, fg #991B1B/#166534 based on toPd vs fromPd) — they 
already apply. So empty cells now show "$0" WITH the correct pink/green color, 
matching the Accounts matrix schema. (Totals/Changes rows already show "$0" — 
leave them as-is.)

=== ITEM 7: Add SM/SUB/DFUL/LOSS monikers in the Detail table ===
The Detail table renders BANK PD (r.pdInitial) and CAS PD (r.pdFinal) as plain 
numbers via out(...). Add the moniker suffix using the EXISTING colLabel 
helper (13->"13 / SM", 14->"14 / SUB", 15->"15 / DFUL", 16->"16 / LOSS", 
others plain).

Apply to BOTH DetailTable AND DetailTableRows (both render the detail rows):
- BANK PD cell: change {out(r.pdInitial ?? NBSP)} to show 
  {r.pdInitial != null ? colLabel(r.pdInitial) : NBSP}
- CAS PD cell: change {out(r.pdFinal ?? NBSP)} to show 
  {r.pdFinal != null ? colLabel(r.pdFinal) : NBSP}

Use the existing colLabel function (do not redefine it). Keep the same cell 
styles/alignment.

CONSTRAINTS:
- Reuse the EXISTING colLabel helper and existing color logic — do not invent 
  new helpers or colors.
- Item 5: only Totals + Changes rows of MatrixCount (body already shows "0").
- Item 6: only MatrixCommitment BODY cells (Totals/Changes already show "$0"); 
  keep their directional colors unchanged.
- Item 7: apply colLabel to BANK PD and CAS PD cells in BOTH DetailTable and 
  DetailTableRows.
- Do NOT change data, calculations, column widths, the header/footer, or any 
  other section.
- Do NOT touch pageSetup.ts, page size, margins, or backend.
- Only edit this one file. Show the three changes (MatrixCount Totals/Changes, 
  MatrixCommitment body cells, Detail table PD cells in both components).

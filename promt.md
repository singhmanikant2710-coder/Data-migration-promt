Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Two changes: hide zeros in colored matrix cells, and shade subreport rows.

PART A — Hide "0"/"$0" in COLORED cells only (Issue 6):
In the data cells of BOTH matrices, the colored cells (upgrade/downgrade, 
where toPd != fromPd) currently show "0"/"$0". Client wants these hidden 
(blank) in colored cells so real changes pop. Keep values in white/diagonal 
cells (toPd == fromPd) unchanged.

The cell already computes bg (backgroundColor: #fee2e2 for toPd>fromPd, 
#dcfce7 for toPd<fromPd, undefined for diagonal). Use this:
- MatrixCount cell value: change {String(v)} to:
    {bg && v === 0 ? "" : String(v)}
  (blank when the cell is colored AND value is 0; otherwise show the number, 
  including 0 on white/diagonal cells)
- MatrixCommitment cell value: change {fmt(v)} to:
    {bg && (!v || v === 0) ? "" : fmt(v)}
  (blank when colored AND zero; otherwise show fmt(v))

So: colored cell + zero -> blank; colored cell + nonzero -> show value; 
white/diagonal cell -> show value (including 0). This makes actual changes 
stand out.

PART B — Shade subreport rows (Issue 7):
In Subreport01_Count ("PD Migration Totals by Account") and 
Subreport02_Commitment ("PD Migration Totals by Commitment"), shade each data 
row based on fromPd vs toPd (both available per row):
- toPd < fromPd (upgrade): row background light green #dcfce7
- toPd > fromPd (downgrade): row background light red #fee2e2
- toPd == fromPd (no change): no background (default white)

Add the backgroundColor to each data row's container <View> (the row-level 
View, so the whole row is shaded). Compute per row:
    const rowBg = r.toPd < r.fromPd ? "#dcfce7" : r.toPd > r.fromPd ? "#fee2e2" : undefined;
Then apply backgroundColor: rowBg to the row's style. Apply to BOTH subreport 
tables.

CONSTRAINTS:
- Item 6: blank zeros ONLY in colored cells (bg defined); keep values in 
  white/diagonal cells. Apply to both matrices.
- Item 7: shade full rows in both subreports using the same colors as the 
  matrices (#dcfce7 green, #fee2e2 red).
- Do NOT change data, calculations, or the % columns in subreports.
- Do NOT touch pageSetup.ts, page size, margins, or backend.
- Only edit this one file. Show the colored-cell zero-hiding (both matrices) 
  and the subreport row shading (both subreports).

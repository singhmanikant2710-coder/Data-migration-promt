Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Restructure the header and add a footer note for BOTH MatrixCount and 
MatrixCommitment. Client-confirmed requirements. Apply identically to both 
matrices.

=== PART 1: Make the header row THINNER ===
The header height comes from padding (6) and lineHeight (1.25) at fontSize 9 
(not from label wrapping). Shrink it so we can keep the SM/SUB/DFUL/LOSS 
labels AND fit a CAS PD grouping row + a footer.

Create a compact header cell style (or add inline overrides) for the header 
cells:
- fontSize: 7 (down from inherited 9)
- padding: 3 (down from 6)
- lineHeight: 1.1 (down from 1.25)
Apply these to the header PD-column cells (and the new CAS PD row) so the 
header is visibly thinner. Keep styles.thDark's background/color/borders.

=== PART 2: Add a "CAS PD" grouping header row ABOVE the 1-16 columns ===
Convert the single header row into TWO header rows:

TOP header row (new):
- First cell (over BANK PD, 8% width): empty or blank (no label), same dark 
  background.
- A single spanning cell over the 1-16 columns (80% width): centered text 
  "CAS PD", dark background, thin/compact styling.
- Last cell (over Totals, 12% width): empty/blank, same dark background.

BOTTOM header row (existing 1-16 labels):
- Keep BANK PD (8%), the 16 colLabel(pd) cells (5% each), and Totals (12%) — 
  but apply the thinner compact styling from Part 1.

Both header rows should be wrapped so they stay together with wrap={false} 
and the header integrity (minPresenceAhead) is preserved. Ensure the top row's 
three segments (8% + 80% + 12%) line up exactly with the bottom row's columns.

=== PART 3: Align "BANK PD" header with the PD rating column ===
The "BANK PD" header cell and the first data column (fromPd numbers) are both 
left-aligned. Center-align BOTH the "BANK PD" header cell AND the first-column 
data cells (the fromPd values) so the header sits directly above and aligned 
with the rating numbers. Add textAlign: "center" to:
- the BANK PD header cell, and
- the first-column data cell (String(r.fromPd)) in the data rows
in BOTH matrices, so they visually align.

=== PART 4: Add a footer NOTE below each matrix ===
Below the table (after the closing of styles.table, still inside the matrix 
component's root View), add a small note. Use this EXACT text (client-
confirmed, "Accounts" not "Scorecards"):

"CAS PD Rating totals represent the committed exposure of Accounts reviewed 
by Bank PD as of the review date. Red cells indicate PD downgrades by CAS; 
green cells indicate PD upgrades. Based on review population, there may not 
be Bank PD rows for every PD rating (1-16)."

Style the note: fontSize 7, color #475569, marginTop 4, italic if easy. 
Full width. Add it below BOTH MatrixCount's and MatrixCommitment's tables.

CONSTRAINTS:
- Apply ALL parts to BOTH MatrixCount and MatrixCommitment identically.
- Keep the SM/SUB/DFUL/LOSS labels (colLabel unchanged) — the thinner header 
  makes room for them.
- Do NOT change the data rows' values, the Changes/%Change rows, colors, 
  calculations, or column widths (8% / 16×5% / 12% stays).
- Keep wrap={false} on the header rows only (per earlier pagination fix — 
  data/summary rows stay without wrap={false}).
- Do NOT touch pageSetup.ts, page size, margins, the page footer, or backend.
- Only edit this one file. Show the new two-row header, the thinner styles, 
  the centered BANK PD alignment, and the footer note — for both matrices.

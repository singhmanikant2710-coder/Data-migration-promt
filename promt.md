Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Two fixes found during verification of #182.

=== FIX A: Scorecard ID wrap (wrong CSS value) ===
The wrapAnywhere style uses wordBreak: "breakAll" — this is a NON-STANDARD 
value and react-pdf ignores it, so long IDs don't wrap. The correct value is 
"break-all" (with a hyphen). Fix the style:
    wrapAnywhere: { wordBreak: "breakAll" }
    ->
    wrapAnywhere: { wordBreak: "break-all" }
This makes the break-anywhere actually apply, so long unspaced Scorecard IDs 
wrap within the 22% column instead of overflowing.

Also, as a safety net, add overflow clipping to the table so nothing paints 
past the border (matching the sibling CrmSummaryTablePDF.tsx which has 
overflow: "hidden"):
    In styles.table, add: overflow: "hidden"
(Keep everything else in styles.table unchanged.)

=== FIX B: "K" (and other glyphs) overlapping the row border in Bank PD/LGD ===
The Bank PD and Bank LGD cells' Text has no explicit lineHeight, so inside the 
vertically-centered tight cell, tall glyphs like "K" overlap the row's border. 
Add an explicit lineHeight to the Text so glyphs sit within the line box.

For the Bank PD and Bank LGD (and CAS PD / CAS LGD if they have the same 
centered Text) DATA cells, add lineHeight to the Text style:
    <Text style={[styles.tdText, { textAlign: "center", width: "100%" }]}>
    ->
    <Text style={[styles.tdText, { textAlign: "center", width: "100%", lineHeight: 1.2 }]}>
Apply this to all four centered PD/LGD data cells (Bank PD, Bank LGD, CAS PD, 
CAS LGD) so the values sit cleanly within the row without overlapping the 
border.

CONSTRAINTS:
- FIX A: wordBreak "breakAll" -> "break-all" in wrapAnywhere; add overflow 
  "hidden" to styles.table.
- FIX B: add lineHeight: 1.2 to the centered Text in the four PD/LGD data cells.
- Do NOT change column widths, the data, padding, or other cells.
- Only edit this one file. Show the corrected wrapAnywhere, the table overflow, 
  and the PD/LGD Text with lineHeight.

Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Center the data values for Bank PD, Bank LGD, CAS PD, CAS LGD in the scorecard 
table. Currently Bank PD/LGD use { textAlign: "center", width: "100%" } (the 
width:100% contributed to a glyph-overlap issue), and CAS PD/LGD are plain 
left-aligned (styles.tdText).

Make all four consistent and safe — use textAlign center WITHOUT width:"100%", 
and add lineHeight so glyphs don't overlap the row border.

For all FOUR cells (Bank PD sc3, Bank LGD sc4, CAS PD sc5, CAS LGD sc6), set 
the Text style to:
    <Text style={[styles.tdText, { textAlign: "center", lineHeight: 1.2 }]}>

Specifically:
- Bank PD (sc3): change { textAlign: "center", width: "100%" } to 
  { textAlign: "center", lineHeight: 1.2 }  (remove width:100%, add lineHeight)
- Bank LGD (sc4): same change (remove width:100%, add lineHeight)
- CAS PD (sc5): change plain styles.tdText to [styles.tdText, { textAlign: "center", lineHeight: 1.2 }]
- CAS LGD (sc6): same as CAS PD

The td container already has alignItems: "center" (View), so the Text will 
center within the cell without needing width:"100%".

CONSTRAINTS:
- All four PD/LGD data cells: textAlign "center" + lineHeight 1.2, NO width:"100%".
- Do NOT add overflow, do NOT change column widths, do NOT touch other cells or 
  the headers.
- Only edit this one file. Show all four updated cells.

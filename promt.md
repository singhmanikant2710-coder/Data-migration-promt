Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Make the bottom Totals row's "CAS PD Totals" label font size consistent with 
the rest of the row (fontSize 10). Currently it uses styles.th (fontSize 9) 
while the other cells in that row use styles.td (fontSize 10), making the 
label slightly smaller.

In BOTH MatrixCount and MatrixCommitment, the bottom Totals row's first cell 
(the "CAS PD Totals" label) currently uses styles.th:
    <Text style={[styles.th, { flexBasis: "8%", flexGrow: 0, flexShrink: 0 }]}>
      CAS PD Totals
    </Text>

Add an explicit fontSize: 10 to match the row (and keep it bold via styles.th's 
fontWeight if present, or add fontWeight: 700):
    <Text style={[styles.th, { flexBasis: "8%", flexGrow: 0, flexShrink: 0, fontSize: 10 }]}>
      CAS PD Totals
    </Text>

CONSTRAINTS:
- ONLY add fontSize: 10 to the "CAS PD Totals" label cell in both matrices' 
  bottom Totals rows.
- Do NOT change widths, alignment, colors, or any other cell.
- Only edit this one file. Show the updated label cell in both matrices.

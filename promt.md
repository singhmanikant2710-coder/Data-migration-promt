Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

ROOT CAUSE CONFIRMED: The working PD Grade report footer places 
<Text render={...}> DIRECTLY inside the fixed footer <View> (no inner View). 
The CRO footer wraps <Text> inside an extra inner <View>, which breaks the 
render prop's page-number context — so the text doesn't render. Remove the 
inner <View> to match PD Grade's working structure.

Replace BOTH CRO footer blocks with this exact structure (Text directly 
inside the fixed footer View, border back on styles.footer — matching PD 
Grade):

  <View style={styles.footer} fixed>
    <Text
      style={{ fontSize: 8, color: "#334155", textAlign: "center" }}
      render={({ pageNumber, totalPages }) => `${title} • Page ${pageNumber} of ${totalPages}`}
    />
  </View>

AND restore the border on styles.footer (since we removed the inner View that 
was carrying it). Add back to styles.footer:
  borderTopWidth: 1,
  borderTopColor: colors.divider,
  borderTopStyle: "solid",

So styles.footer becomes:
  footer: {
    position: "absolute",
    left: MARGINS.left,
    right: MARGINS.right,
    bottom: MARGINS.bottom,
    height: 48,
    paddingTop: 10,
    borderTopWidth: 1,
    borderTopColor: colors.divider,
    borderTopStyle: "solid",
  }

CONSTRAINTS:
- The KEY fix: <Text> must be a DIRECT child of <View style={styles.footer} fixed> 
  with NO inner <View> between them (exactly like the working PD Grade footer).
- Apply to BOTH footer occurrences.
- Use `title` in scope in both locations.
- Do NOT touch pageSetup.ts, page size, orientation, margins, styles.page, 
  header, or tables.
- Only edit this one file. Show both final footer blocks and confirm there 
  is NO inner View between the fixed footer View and the Text.

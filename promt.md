Single-file edit (DIAGNOSTIC TEST): 
frontend/src/components/pdf/CroProductionSummaryPDF.tsx

The footer <Text> is well-formed but still not visible. I need to determine 
whether the footer is (a) not rendering at all, or (b) rendering but 
positioned off-screen/clipped/invisible.

TEMPORARY DIAGNOSTIC: In the FIRST footer only (inside CroProductionSummaryPage), 
make it impossible to miss by temporarily:
- changing the text color to bright red ("#FF0000")
- increasing fontSize to 16
- adding a visible background: backgroundColor: "#FFFF00" (yellow) on the 
  <View style={styles.footer}>
- adding a static hardcoded prefix so we know it's THIS text:
  render so it outputs: `FOOTERTEST ${title} • Page ${pageNumber} of ${totalPages}`

So the first footer becomes:
  <View style={[styles.footer, { backgroundColor: "#FFFF00" }]} fixed>
    <Text
      style={{ fontSize: 16, color: "#FF0000", textAlign: "center" }}
      render={({ pageNumber, totalPages }) => `FOOTERTEST ${title} • Page ${pageNumber} of ${totalPages}`}
    />
  </View>

Do NOT change anything else. This is a temporary visibility test — I will 
revert the colors/size after confirming. Show the changed block.

Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

CONFIRMED: Static <Text> renders in the CRO footer, but the `render` prop 
callback (for pageNumber/totalPages) does NOT fire in this document structure. 
We need the page number without relying on the top-level render prop.

Try this pattern in the FIRST footer (keep yellow bg + red for the test): 
put the static report name as normal text, and use a NESTED <Text> with 
render ONLY for the page number (react-pdf sometimes fires render on a nested 
page-number Text even when a top-level render fails):

  <View style={[styles.footer, { backgroundColor: "#FFFF00" }]} fixed>
    <Text style={{ fontSize: 12, color: "#FF0000", textAlign: "center" }}>
      CRO Review Production • Page <Text render={({ pageNumber, totalPages }) => `${pageNumber} of ${totalPages}`} />
    </Text>
  </View>

Generate/save.
- If it shows "CRO Review Production • Page 1 of 5" → this nested pattern 
  works; we'll apply it (cleaned up) to both footers.
- If it shows "CRO Review Production • Page " (page number missing) → even 
  nested render fails; we'll fall back to static-only footer (report name, 
  no page number) OR investigate the Page structure further.

Show the changed block and the result.

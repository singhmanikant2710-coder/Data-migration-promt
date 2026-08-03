Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

DIAGNOSIS CONFIRMED: The footer <View> renders (yellow background is visible) 
but the <Text render={...}> produces NO text. The render prop on <Text> is 
not outputting in this context.

FIX: In react-pdf, the `render` callback should be used differently. Change 
BOTH footers to use the render prop as a CHILD of <Text> (function-as-child / 
render inside), OR move `render` onto the <Text> with children. The reliable 
pattern that works with page-level pageNumber/totalPages is:

Keep the diagnostic first footer for now but change its Text to this form:
  <View style={[styles.footer, { backgroundColor: "#FFFF00" }]} fixed>
    <Text
      render={({ pageNumber, totalPages }) => `FOOTERTEST ${title} • Page ${pageNumber} of ${totalPages}`}
      style={{ fontSize: 16, color: "#FF0000", textAlign: "center" }}
    />
  </View>

If that STILL shows no text, instead try wrapping render output as children:
  <View style={[styles.footer, { backgroundColor: "#FFFF00" }]} fixed>
    <Text style={{ fontSize: 16, color: "#FF0000", textAlign: "center" }}>
      <Text render={({ pageNumber, totalPages }) => `FOOTERTEST ${title} • Page ${pageNumber} of ${totalPages}`} />
    </Text>
  </View>

BEFORE editing, IMPORTANT — investigate the working CRM Summary footer to 
copy its EXACT working pattern:
1. Open CrmSummaryPDF.tsx and show the EXACT footer <Text> including whether 
   `render` is a prop, the order of props, and whether there's anything 
   different (e.g. `fixed` on the Text itself, or a different react-pdf 
   import/version).
2. Check the react-pdf import in both files — are they importing Text from 
   the same @react-pdf/renderer version? Show both import lines.
3. Show whether CRM's footer <Text> has `fixed` on it, or only on the 
   parent View.

Report those findings AND apply whichever pattern matches CRM's working 
footer to the CRO first footer (keep yellow/red diagnostic styling for now). 
Show the final first-footer block.

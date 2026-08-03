Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

The footer text is not rendering in the generated PDF. The title wiring and 
padding are confirmed correct, but the footer's <Text> element structure 
appears malformed/broken (it is not rendering). 

Fix by replacing BOTH footer occurrences with the EXACT working structure 
used by the CRM Summary report (CrmSummaryPDF.tsx), which renders correctly. 
Use an inner <View> wrapper containing the <Text>, like this:

  <View style={styles.footer} fixed>
    <View style={{ borderTopWidth: 1, borderTopColor: colors.divider, borderTopStyle: "solid", paddingTop: 4 }}>
      <Text
        style={{ fontSize: 8, color: "#334155", textAlign: "center" }}
        render={({ pageNumber, totalPages }) => `${title} • Page ${pageNumber} of ${totalPages}`}
      />
    </View>
  </View>

Apply this EXACT structure to BOTH footer occurrences (the one inside 
CroProductionSummaryPage and the one inside the second Page of the default 
export). Ensure:
- The <Text> is a proper self-closed element with style and render props 
  (not malformed).
- Uses the `title` variable already in scope in each location.
- Move the border from styles.footer's container into this inner <View> 
  wrapper (matching CRM's approach), OR keep styles.footer's border — but 
  ensure the <Text> actually renders. If styles.footer already has a 
  borderTop, you may remove the inner wrapper's borderTop to avoid a double 
  border; the KEY requirement is that the <Text> renders. Prefer matching 
  CRM's exact working pattern.

CONSTRAINTS:
- Do NOT touch pageSetup.ts, page size, orientation, margins, styles.page 
  paddingBottom, or any header/table content.
- Only fix the two footer blocks so the centered "${title} • Page X of Y" 
  text actually renders.
- Verify the JSX is syntactically valid (proper <Text ... /> and closing 
  </View> tags).
- Only edit this one file. Show both final footer blocks verbatim so I can 
  confirm the <Text> is well-formed.

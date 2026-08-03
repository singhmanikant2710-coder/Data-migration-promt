Single-file edit: frontend/src/components/pdf/CroProductionSummaryPDF.tsx

ROOT CAUSE FOUND: The CRM footer (which works) wraps its <Text> inside an 
INNER <View>, while the CRO footer places <Text> directly under the 
<View style={styles.footer} fixed> with no inner View. In react-pdf, a 
<Text render={...}> directly under a fixed View can fail to render the 
page-context text; the inner <View> wrapper (as CRM uses) fixes this.

FIX: Update BOTH CRO footers to match CRM's exact working structure — add an 
inner <View> wrapper around the <Text>. Also REMOVE the temporary diagnostic 
yellow background and red/16pt styling, restoring normal footer styling.

Replace BOTH footer blocks with this final version:

  <View style={styles.footer} fixed>
    <View style={{ borderTopWidth: 1, borderTopColor: colors.divider, borderTopStyle: "solid", paddingTop: 4 }}>
      <Text
        style={{ fontSize: 8, color: "#334155", textAlign: "center" }}
        render={({ pageNumber, totalPages }) => `${title} • Page ${pageNumber} of ${totalPages}`}
      />
    </View>
  </View>

Notes:
- The inner <View> is the key fix (matches CRM's working pattern).
- Since the inner <View> now carries the top border, and styles.footer ALSO 
  has a borderTop, remove the borderTop from styles.footer to avoid a double 
  line — OR remove the border from the inner View and rely on styles.footer's 
  border. Pick ONE border source (prefer keeping the inner View's border like 
  CRM, and remove borderTopWidth/borderTopColor/borderTopStyle from 
  styles.footer). Ensure only ONE border line renders.
- Restore normal styling: fontSize 8, color #334155, no yellow background, 
  no FOOTERTEST prefix.
- Use `title` in scope in both locations.

CONSTRAINTS:
- Do NOT touch pageSetup.ts, page size, orientation, margins, styles.page, 
  header, or tables.
- Apply the inner-View wrapper to BOTH footers.
- Only edit this one file. Show both final footer blocks verbatim and 
  confirm the styles.footer border was deduplicated (only one border line).

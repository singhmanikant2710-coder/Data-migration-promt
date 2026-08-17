Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

The last-page block now has TWO heading lines by mistake — the old one wasn't removed, a new one was added. Remove the OLD heading line so only "APPLIED REPORT FILTERS" remains.

Current state (both lines present):
<Text style={styles.sectionTitle}>Current Filter Payload</Text>
<Text style={styles.sectionTitle}>APPLIED REPORT FILTERS</Text>

Remove ONLY this line:
<Text style={styles.sectionTitle}>Current Filter Payload</Text>

Result should be exactly:
<View>
  <Text style={styles.sectionTitle}>APPLIED REPORT FILTERS</Text>
  <Text style={{ fontSize: 8, color: "#0f172a" }}>
    {buildFilterParagraph("crm-summary-table", props?.meta?.filters)}
  </Text>
</View>

CONSTRAINTS:
- Remove ONLY the "Current Filter Payload" <Text> line.
- Keep the "APPLIED REPORT FILTERS" line and buildFilterParagraph exactly as-is.
- Do NOT touch anything else.
- Show the diff.

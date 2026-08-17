Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix (Geoff Item 5): Move the "Applied Report Filters" payload from its own separate <Page> to directly below the detail tables, inside ConsolidatedFindingsObservationsPage's auto-flowing Page. Then remove the now-empty separate filter Page.

STEP 1 — Add `filters` to ConsolidatedFindingsObservationsPage's props.
BEFORE:
function ConsolidatedFindingsObservationsPage({
  items = [],
  title,
  generatedOn
}: {
  items: CrmFindingsObservationsPdfItem[];
  title: string;
  generatedOn: string;
}) {
AFTER:
function ConsolidatedFindingsObservationsPage({
  items = [],
  title,
  generatedOn,
  filters
}: {
  items: CrmFindingsObservationsPdfItem[];
  title: string;
  generatedOn: string;
  filters?: any;
}) {

STEP 2 — Inside this Page, add the filter block AFTER the sortedComponents map/content and BEFORE the fixed footer View. Add a top margin so it doesn't stick to the tables. Use wrap={false} so it isn't split across pages.

Insert this block immediately before the `<View style={styles.footer} fixed>` line:

<View wrap={false} style={{ marginTop: 16 }}>
  <Text style={styles.sectionTitle}>Applied Report Filters</Text>
  <Text style={{ fontSize: 8, color: "#0f172a" }}>
    {buildFilterParagraph("crm-findings-observations", filters)}
  </Text>
</View>

STEP 3 — In the items-present branch return (the main Document), remove the ENTIRE separate filter <Page> (the one containing "Applied Report Filters" + buildFilterParagraph + its footer), and pass filters to ConsolidatedFindingsObservationsPage.

BEFORE:
<Document>
  <ConsolidatedFindingsObservationsPage items={items as any} title={title} generatedOn={genOn} />
  <Page size={PAGE_SIZE} orientation={PAGE_ORIENTATION} style={styles.page}>
    <View>
      <Text style={styles.sectionTitle}>Applied Report Filters</Text>
      <Text style={{ fontSize: 8, color: "#0f172a" }}>
        {buildFilterParagraph("crm-findings-observations", filters)}
      </Text>
    </View>
    <View style={styles.footer} fixed>
      ...
    </View>
  </Page>
</Document>

AFTER:
<Document>
  <ConsolidatedFindingsObservationsPage items={items as any} title={title} generatedOn={genOn} filters={filters} />
</Document>

CONSTRAINTS:
- Do NOT touch the no-items branch in this edit (we'll handle it separately after verifying this one).
- Do NOT change the tables, grouping logic, headers, or the existing fixed footer inside ConsolidatedFindingsObservationsPage.
- Ensure `filters` is available in the outer scope where ConsolidatedFindingsObservationsPage is called (it's already used in the old filter Page, so it exists).
- Remove the separate filter <Page> completely and cleanly — no dangling JSX.
- Do NOT touch any other file.
- Show the FULL diff.

Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix (Item 4, has-items branch): Rename "Current Filter Payload" -> "Applied Report Filters" and move it directly below the details table (into the last detail page, after the table, before that page's footer) instead of on its own separate page. Then remove the separate filter Page.

=== STEP 1 — Thread filters + isLast into detail pages ===

Update DetailTablePage to accept filters and isLast, and render the filter block after the table (before footer) ONLY on the last page:

BEFORE:
function DetailTablePage({ rows }: { rows: CrmPdGradeMigrationDetailRow[] }) {
  return (
    <Page size={PAGE_SIZE} orientation={PAGE_ORIENTATION} style={styles.page}>
      <View style={styles.headerBar}>
        <Text style={styles.headerTitle}>CRM PD Grade Migration</Text>
        <Text style={styles.headerMeta}>{out(formatRunDate())}</Text>
      </View>
      <Text style={styles.sectionTitle}>Detail</Text>
      <View style={styles.table}>
        <DetailTableHeader />
        <DetailTableRows rows={rows} />
      </View>
      <View style={styles.footer} fixed>
        <Text
          style={{ fontSize: 9, color: "#0F172A", textAlign: "center" }}
          render={({ pageNumber, totalPages }) => `CRM PD Grade Migration · Page ${pageNumber} of ${totalPages}`}
        />
      </View>
    </Page>
  );
}

AFTER:
function DetailTablePage({ rows, filters, isLast }: { rows: CrmPdGradeMigrationDetailRow[]; filters?: any; isLast?: boolean }) {
  return (
    <Page size={PAGE_SIZE} orientation={PAGE_ORIENTATION} style={styles.page}>
      <View style={styles.headerBar}>
        <Text style={styles.headerTitle}>CRM PD Grade Migration</Text>
        <Text style={styles.headerMeta}>{out(formatRunDate())}</Text>
      </View>
      <Text style={styles.sectionTitle}>Detail</Text>
      <View style={styles.table}>
        <DetailTableHeader />
        <DetailTableRows rows={rows} />
      </View>
      {isLast ? (
        <View wrap={false} style={{ marginTop: 16 }}>
          <Text style={styles.sectionTitle}>Applied Report Filters</Text>
          <Text style={{ fontSize: 8, color: "#0f172a" }}>
            {buildFilterParagraph("crm-pd-grade-migration", filters)}
          </Text>
        </View>
      ) : null}
      <View style={styles.footer} fixed>
        <Text
          style={{ fontSize: 9, color: "#0F172A", textAlign: "center" }}
          render={({ pageNumber, totalPages }) => `CRM PD Grade Migration · Page ${pageNumber} of ${totalPages}`}
        />
      </View>
    </Page>
  );
}

=== STEP 2 — Pass filters + isLast from DetailTablePages ===

BEFORE:
function DetailTablePages({ items }: { items: CrmPdGradeMigrationDetailRow[] }) {
  const rowsPerPage = 22;
  const groups = chunk(items || [], rowsPerPage);
  return (
    <>
      {groups.map((g, i) => <DetailTablePage key={`dtp-${i}`} rows={g} />)}
    </>
  );
}

AFTER:
function DetailTablePages({ items, filters }: { items: CrmPdGradeMigrationDetailRow[]; filters?: any }) {
  const rowsPerPage = 22;
  const groups = chunk(items || [], rowsPerPage);
  return (
    <>
      {groups.map((g, i) => (
        <DetailTablePage key={`dtp-${i}`} rows={g} filters={filters} isLast={i === groups.length - 1} />
      ))}
    </>
  );
}

=== STEP 3 — Remove the separate filter Page and pass filters to DetailTablePages (has-items branch) ===

BEFORE:
    <DetailTablePages items={data?.items || []} />

    {/* Final page: payload echo */}
    <Page size={PAGE_SIZE} orientation={PAGE_ORIENTATION} style={styles.page}>
      <View>
        <Text style={styles.sectionTitle}>Current Filter Payload</Text>
        <Text style={{ fontSize: 8, color: "#0f172a" }}>
          {buildFilterParagraph("crm-pd-grade-migration", filters)}
        </Text>
      </View>
      <View style={styles.footer} fixed>
        <Text
          style={{ fontSize: 9, color: "#0F172A", textAlign: "center" }}
          render={({ pageNumber, totalPages }) => `CRM PD Grade Migration · Page ${pageNumber} of ${totalPages}`}
        />
      </View>
    </Page>

AFTER:
    <DetailTablePages items={data?.items || []} filters={filters} />

CONSTRAINTS:
- Do NOT touch the no-items branch in this edit (handle separately after verifying).
- Do NOT change the detail table, chunking (22), header, or footer logic.
- Keep wrap={false} on the filter block so it stays intact (moves to next page if it doesn't fit).
- Remove the separate filter Page completely and cleanly.
- Do NOT touch any other file.
- Show the FULL diff.

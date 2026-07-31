Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

GOAL (Geoff comment): In the CRM PD Grade Migration report footer, remove 
the small FHN logo on the left, and reformat the footer to mirror the CRM 
Summary Table report's footer style — a single CENTERED line reading 
"<Report Name> • Page X of Y", with the top border kept. No logo, no 
"CAS RiskReview" brand text.

The current footer (repeated in ~5 places) is:
    <View style={styles.footerLeft}>
      <Image src="/assets/FHB_Logo.png" style={styles.footerLogo} />
      <Text style={styles.footerBrand}>CAS RiskReview</Text>
    </View>
    <Text
      style={styles.footerText}
      render={({ pageNumber, totalPages }) => `Page ${pageNumber} of ${totalPages}`}
    />

Replace the footer CONTENT in EVERY footer occurrence in this file (the 
diagnostics identified 5: CrmPdGradeMigrationPage; the no-items Document 
Page 1 "No records"; the no-items Document Page 2 "Current Filter Payload"; 
DetailTablePage; and the has-items Document final "Current Filter Payload" 
page).

For each footer, remove the logo+brand <View> and the right-aligned page 
<Text>, and replace with a single centered line:
    <Text
      style={{ fontSize: 9, color: "#0F172A", textAlign: "center" }}
      render={({ pageNumber, totalPages }) => `CRM PD Grade Migration • Page ${pageNumber} of ${totalPages}`}
    />

Report name source per location:
- In CrmPdGradeMigrationPage: a `title` variable is in scope — use it:
      `${title} • Page ${pageNumber} of ${totalPages}`
- In the no-items Document pages: a `t` variable is in scope — use it:
      `${t} • Page ${pageNumber} of ${totalPages}`
- In DetailTablePage and any page where no title variable is in scope: use 
  the literal string "CRM PD Grade Migration" (do NOT thread a new prop):
      `CRM PD Grade Migration • Page ${pageNumber} of ${totalPages}`

CONSTRAINTS:
- Keep the outer <View style={styles.footer} fixed> wrapper and its 
  border-top styling exactly as-is — only change the INNER content 
  (remove logo/brand/right-text, add the centered line).
- Remove the <Image ... FHB_Logo.png ... /> from ALL footer blocks. Do NOT 
  remove the logo from the report HEADER (only the footer).
- Do NOT change styles.footer, page layout, headers, tables, charts, or any 
  data.
- styles.footerLeft / footerBrand / footerLogo / footerText tokens may 
  become unused — that is fine; leave them defined, do not delete.
- Only edit this one file. List every footer location changed and confirm 
  the FHN logo no longer appears in any footer.

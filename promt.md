Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Two changes to leave exactly ONE filter payload — on the last page, with the "APPLIED REPORT FILTERS" header.

=== CHANGE 1 — Remove the first-page inline filter block entirely ===
Inside CrmSummaryTablePage's returned <Page>, remove this ENTIRE block including the comment, the {filters ? ...} wrapper, and the closing ) : null}:

REMOVE (exactly this, whole block):
{/* Applied Filters Section (optional) */}
{filters ? (
<View>
<Text style={styles.sectionTitle}>APPLIED REPORT FILTERS</Text>
<View style={{ marginBottom: 8 }}>
<Text style={{ fontSize: 8, color: "#0f172a" }}>
{`REPORT NAME: ${out(title)}, SAMPLE ID: ${out(String(filters?.sampleId ?? ""))}, REVIEW STATUS: ${out(String(filters?.reviewStatus ?? ""))}, START DATE: ${out(String(filters?.startDate ?? ""))}, END DATE: ${out(String(filters?.endDate ?? ""))}, SEGMENT: ${filters?.segment ?? "No Selection"}, MARKET: ${filters?.market ?? "No Selection"}, RELATIONSHIP MANAGER: ${filters?.relationshipManager ?? "No Selection"}, UNIT: ${filters?.unit ?? "No Selection"}, PORTFOLIO MANAGER: ${filters?.portfolioManager ?? "No Selection"}, PORTFOLIO INDUSTRY: ${filters?.portfolioIndustry ?? "No Selection"}, PORTFOLIO CLASSIFICATION: ${filters?.portfolioClassification ?? "No Selection"}, PORTFOLIO SEGMENT: ${filters?.portfolioSegment ?? "No Selection"}, SPECIAL ASSETS: ${filters?.specialAssets == null ? "No Selection" : (filters?.specialAssets ? "Yes" : "No")}, CENTRALIZED COMMERCIAL (CCL): ${filters?.ccl == null ? "No Selection" : (filters?.ccl ? "Yes" : "No")}, REVIEWER: ${filters?.croName ?? "No Selection"}`}
</Text>
</View>
</View>
) : null}

=== CHANGE 2 — Rename the last-page heading ===
In the separate <Page> in the default export:

BEFORE:
<Text style={styles.sectionTitle}>Current Filter Payload</Text>

AFTER:
<Text style={styles.sectionTitle}>APPLIED REPORT FILTERS</Text>

CONSTRAINTS:
- CHANGE 1: remove the whole inline block cleanly — comment + {filters ? ( ... ) : null}. No dangling braces, parens, or JSX. The surrounding <Page> content must remain valid.
- CHANGE 2: change ONLY the heading text; keep buildFilterParagraph and everything else in that block unchanged.
- Do NOT touch the footer, the buildFilterParagraph call, styles, or any other section.
- Do NOT change any other file.
- Show the FULL diff so I can confirm the removal is clean and nothing else shifted.

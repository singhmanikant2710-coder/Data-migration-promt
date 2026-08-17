Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Fix: The CRM Component sub-headers above each table (e.g. "RISK RECOGNITION (4)", "SCORECARD MANAGEMENT (5)") should be fontSize 11. They currently use the SHARED styles.sectionTitle (fontSize 10), which is also used by "APPLIED REPORT FILTERS" and the "Current Filter Payload" heading. To avoid affecting those, add a NEW style just for component sub-headers.

STEP 1 — Add a new style `componentTitle` right after the sectionTitle definition. It is a copy of sectionTitle with fontSize 11. Do NOT modify sectionTitle.

componentTitle: {
  fontSize: 11,
  fontWeight: 700,
  color: "#1F3864",
  marginBottom: 6,
  borderBottomWidth: 1,
  borderBottomColor: "#cbd5e1",
  borderBottomStyle: "solid",
  paddingBottom: 3,
  textTransform: "uppercase"
},

STEP 2 — Update ONLY the component sub-header JSX to use it.

BEFORE:
<Text style={styles.sectionTitle}>{out(`${cleanCompName(sec?.component)} (${count})`)}</Text>

AFTER:
<Text style={styles.componentTitle}>{out(`${cleanCompName(sec?.component)} (${count})`)}</Text>

CONSTRAINTS:
- ONLY add the componentTitle style and change this one <Text> to use it.
- Do NOT modify sectionTitle.
- Do NOT change the "APPLIED REPORT FILTERS" or "Current Filter Payload" headings — they stay on sectionTitle.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

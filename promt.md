Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Two small changes in the header only.

1. Change the header LEFT title default from "CRM Summary Table" to "CRM Findings Summary Table".
2. Change the header RIGHT text from the caption to the run date (current date), matching the pattern other reports use. formatDate is already imported.

BEFORE:
<View style={styles.headerBar}>
  <Text style={styles.headerTitle}>{title || "CRM Summary Table"}</Text>
  <Text style={styles.headerMeta}>{out(caption)}</Text>
</View>

AFTER:
<View style={styles.headerBar}>
  <Text style={styles.headerTitle}>{title || "CRM Findings Summary Table"}</Text>
  <Text style={styles.headerMeta}>{out(formatDate(new Date() as any))}</Text>
</View>

CONSTRAINTS:
- ONLY change these two lines in the header (headerBar block).
- Do NOT change headerBar / headerTitle / headerMeta styles.
- Do NOT touch the footer, caption variable, or any other block yet.
- Do NOT change any other file.
- If `title` is passed in as a prop with an old value, only change the DEFAULT fallback string here — do not alter how title is received.
- Only edit this one file. Show the diff.

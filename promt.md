Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Two header-only changes.

CONTEXT: formatDate (already imported) accepts an ISO string like "2026-05-01" and returns M/D/YYYY. It does NOT accept a Date object (returns blank). So pass an ISO string via new Date().toISOString().

BEFORE:
<View style={styles.headerBar}>
  <Text style={styles.headerTitle}>{title || "CRM Summary Table"}</Text>
  <Text style={styles.headerMeta}>{out(caption)}</Text>
</View>

AFTER:
<View style={styles.headerBar}>
  <Text style={styles.headerTitle}>{title || "CRM Findings Summary Table"}</Text>
  <Text style={styles.headerMeta}>{out(formatDate(new Date().toISOString()))}</Text>
</View>

CONSTRAINTS:
- ONLY change these two lines inside the headerBar block.
- Do NOT change headerBar / headerTitle / headerMeta styles.
- Do NOT modify the footer, caption variable, or any other block.
- Do NOT change any other file.
- Only edit this one file. Show the diff.

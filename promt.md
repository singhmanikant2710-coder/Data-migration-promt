Two changes in one file: frontend/src/components/pdf/CrmSummaryPDF.tsx

These are independent, non-shared edits.

=== CHANGE 1 — Header font size (match other reports: 14) ===
Style `headerTitle` is local to this file. Change fontSize only.

BEFORE:
headerTitle: {
  color: colors.bannerText,
  fontSize: 20,
  fontWeight: 700
}

AFTER:
headerTitle: {
  color: colors.bannerText,
  fontSize: 14,
  fontWeight: 700
}

=== CHANGE 2 — Rename heading "Current Filter Payload" -> "Applied Report Filters" ===
This string appears TWICE in CrmSummaryPDF.tsx (the no-items Document branch AND the normal Document final page). Rename BOTH occurrences. It is a plain display heading only — not a key used in data/filtering/logic.

BEFORE (both occurrences):
<Text style={styles.sectionTitle}>Current Filter Payload</Text>

AFTER (both occurrences):
<Text style={styles.sectionTitle}>Applied Report Filters</Text>

CONSTRAINTS:
- ONLY change: (a) fontSize 20->14 in headerTitle, (b) the two "Current Filter Payload" heading strings.
- Do NOT change color, fontWeight, or any other property of headerTitle.
- Do NOT touch the sectionTitle style itself, only the text inside the two <Text> elements.
- Do NOT rename "Current Filter Payload" in CrmPdGradeMigrationPDF.tsx — that file is out of scope for this ticket.
- Do NOT touch buildFilterParagraph or any other file.
- Only edit this one file. Show the diff.

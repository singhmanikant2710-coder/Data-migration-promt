Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Fix (Item 1): The section headings (light-red highlighted titles like "PD GRADE MIGRATION BY NUMBER OF ACCOUNTS", "PD MIGRATION TOTALS BY ACCOUNT", "FINAL PD DISTRIBUTION", "DETAIL", etc.) should be font size 11. These all use styles.sectionTitle (currently fontSize 10). Change it to 11.

BEFORE (sectionTitle style): fontSize: 10
AFTER  (sectionTitle style): fontSize: 11

CONSTRAINTS:
- ONLY change fontSize 10 -> 11 in the sectionTitle style object.
- Do NOT change fontWeight, color, marginBottom, borders, paddingBottom, textTransform, or anything else.
- Do NOT touch any other style (headerTitle, th, thDark, td, footer, etc.).
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

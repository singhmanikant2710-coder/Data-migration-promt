Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Bug #181 header fix — match the PD Migration report: title "CRM Findings 
Summary Table" on the LEFT, report run date on the RIGHT (currently the right 
shows the caption/Sample Name, which is wrong).

CHANGE 1 — Title text default:
    <Text style={styles.headerTitle}>{title || "CRM Summary Table"}</Text>
    ->
    <Text style={styles.headerTitle}>{title || "CRM Findings Summary Table"}</Text>

CHANGE 2 — Right side: run date instead of caption. Currently:
    <Text style={styles.headerMeta}>{out(caption)}</Text>
Change to show the formatted run date (like PD Migration uses formatRunDate):
    <Text style={styles.headerMeta}>{out(formatRunDate(genOn))}</Text>
- Import formatRunDate from pageSetup if not already imported.
- Resolve genOn from the same prop PD Migration uses (check props for genOn / 
  generatedOn / meta.generatedOn). Show how you obtained the date.

Keep the headerBar layout (row, space-between, title left / meta right) 
unchanged — it already matches PD Migration.

CONSTRAINTS:
- Title default -> "CRM Findings Summary Table".
- Right meta -> run date via formatRunDate (not caption/Sample Name).
- Use the SAME date source + formatRunDate helper as PD Migration.
- Do NOT touch footer, table, or filter sections yet.
- Only edit this one file. Show the header changes + how genOn is resolved.

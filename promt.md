Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Bug #181 — fix the header to match the PD Migration report: title "CRM Findings 
Summary Table" on the LEFT, report run date on the RIGHT (currently the right 
side shows the caption/Sample Name, which is wrong).

Reference (PD Migration, CrmPdGradeMigrationPDF.tsx): 
    const hdrRight = formatRunDate(genOn);
    <View style={styles.headerBar}>
      <Text style={styles.headerTitle}>{title}</Text>
      <Text style={styles.headerMeta}>{out(hdrRight)}</Text>
    </View>

Apply the same to CrmSummaryTablePDF.tsx:

CHANGE 1 — Title text: The title currently defaults to "CRM Summary Table". 
Change the default to "CRM Findings Summary Table":
    <Text style={styles.headerTitle}>{title || "CRM Summary Table"}</Text>
    ->
    <Text style={styles.headerTitle}>{title || "CRM Findings Summary Table"}</Text>

CHANGE 2 — Right side shows RUN DATE, not caption: The header meta currently 
shows {out(caption)} (the reportingCaption / Sample Name). Change it to show 
the formatted run date, matching PD Migration.
- This report receives a generated-on date (check props for genOn / generatedOn 
  / meta.generatedOn — the same source PD Migration uses via formatRunDate). 
  Use formatRunDate on that date.
- Change:
    <Text style={styles.headerMeta}>{out(caption)}</Text>
  to:
    <Text style={styles.headerMeta}>{out(formatRunDate(genOn))}</Text>
  (Import formatRunDate from pageSetup if not already imported, and resolve 
  genOn from the same prop PD Migration uses — show how you obtained the date.)

The layout (headerBar: row, justifyContent space-between; title left, meta 
right) already matches PD Migration — keep it.

CONSTRAINTS:
- Title default -> "CRM Findings Summary Table".
- Right-side meta -> run date via formatRunDate (not the caption/Sample Name).
- Keep headerBar layout (left title, right date) unchanged.
- Use the SAME date source and formatRunDate helper as PD Migration.
- Do NOT change the footer or other sections yet.
- Only edit this one file. Show the updated header (title + run date) and how 
  genOn is resolved.

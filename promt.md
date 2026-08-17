Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Fix: The Scorecard ID value still overflows its cell. Two issues: (a) the \u200B soft-break is not reliably respected by react-pdf, and (b) the value isn't wrapped in a <Text> with a break style. Replace the fragile soft-break approach with a dedicated break-all style applied ONLY to this cell.

STEP 1 — Add a new style next to tdText (do NOT modify tdText):
tdTextId: {
  color: colors.text,
  wordBreak: "break-all"
},

STEP 2 — Update ONLY the scorecardId value cell.

BEFORE:
<View style={[styles.td, styles.sc1]}>
  {out(row?.scorecardId).replace(/(-&)/g, "$1\u200B")}
</View>

AFTER:
<View style={[styles.td, styles.sc1]}>
  <Text style={styles.tdTextId}>{out(row?.scorecardId)}</Text>
</View>

CONSTRAINTS:
- ONLY add the new tdTextId style and update this one scorecardId cell.
- Do NOT modify tdText (all other cells sc2–sc8 must stay exactly as-is).
- Do NOT change any column widths (sc1–sc8 stay as they are now).
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

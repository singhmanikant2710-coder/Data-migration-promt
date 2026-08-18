Single-file edit: frontend/src/components/pdf/ScorecardResultsPDF.tsx

Fix: The Scorecard ID breaks char-by-char / at every hyphen because softBreak() explicitly inserts "\n" after each hyphen (and formatScorecardId inserts "\n" between bank/system IDs). The column (COL8_B = 32%) is already wide enough. Remove the forced line breaks and let it wrap naturally with break-all only when needed.

STEP 1 — Stop inserting "\n" in the value cell. Render the raw id (from formatScorecardId) without softBreak.

BEFORE:
<View style={[styles.td, { width: COL8_B, paddingLeft: 4, paddingRight: 4 }]}>
  <Text style={styles.tdText}>{softBreak(r?.id as any)}</Text>
</View>

AFTER:
<View style={[styles.td, { width: COL8_B, paddingLeft: 4, paddingRight: 4 }]}>
  <Text style={[styles.tdText, { wordBreak: "break-all" }]}>{out(r?.id as any)}</Text>
</View>

(Use `out(...)` to safely handle null/empty — confirm `out` is imported in this file, it is used elsewhere. If not, use `{r?.id ?? ""}`.)

STEP 2 — In formatScorecardId, replace the "\n" between bank and system with a plain space so it doesn't force a hard break either.

BEFORE:
  if (bank && system && bank !== system) {
    return `${bank}\n${system}`;
  }
AFTER:
  if (bank && system && bank !== system) {
    return `${bank} ${system}`;
  }

CONSTRAINTS:
- Remove the softBreak() call on the Scorecard ID value only. Do NOT change softBreak's definition (it may be used elsewhere — check; if used ONLY here, leaving it unused is fine, do not delete in this edit).
- Add wordBreak: "break-all" inline ONLY on this Scorecard ID Text (not on shared tdText style, which other columns use).
- Do NOT change COL8_B width or other columns.
- Do NOT touch any other file.
- Show the diff. Confirm whether softBreak is used anywhere else.

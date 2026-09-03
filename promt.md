Bug 197 — READ-ONLY diagnosis. NO edits. One pass, answer, STOP.

ISSUE (reported by Geoff): Scorecard IDs are 36-40 char GUIDs (e.g. 3ecb14bf-c8d7-4594-8a1a-9ec57e2d7754). In PDF table cells they overflow/bleed across adjacent columns, making neighboring fields illegible. Affected reports: Initial Memo, Final Memo, CAS Linesheet, CRM Summary.

This is a @react-pdf/renderer text-wrapping issue in table cells.

Do exactly this:
1. Grep the frontend PDF components for where Scorecard ID (search: "Scorecard", "scorecardId", "ScorecardId", or the GUID field name) is rendered inside a table cell, across: InitialMemo, FinalMemo, CAS Linesheet (CasLinesheet), CrmSummary PDF components. List each file + line where the Scorecard ID <Text> is rendered.
2. For each, paste the exact <View>/<Text> JSX for that cell AND the StyleSheet style applied to it (width, padding, flex, wordBreak, etc.).
3. Report the column/cell width the Scorecard ID sits in, and whether any wordBreak / overflow / maxWidth style is currently set on it.
4. Confirm whether these components share a common cell/table style or each defines its own.

Report file paths + line numbers + the exact styles. Do NOT propose or write any fix yet.

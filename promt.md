
Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Bug #182 — two simple changes:

CHANGE 1 (header font 20 -> 14): The "CRM Summary" report header title 
currently has fontSize 20. Change it to 14. Keep the right-side run date 
unchanged.
Find the headerTitle style (fontSize: 20) and change to fontSize: 14.

CHANGE 2 (filter heading rename): On the final page, the heading "Current 
Filter Payload" should read "Applied Report Filters". Change ONLY the heading 
text:
    <Text style={styles.sectionTitle}>Current Filter Payload</Text>
    ->
    <Text style={styles.sectionTitle}>Applied Report Filters</Text>
Keep the filter data underneath unchanged.

CONSTRAINTS:
- Only change headerTitle fontSize 20 -> 14, and the "Current Filter Payload" 
  heading text -> "Applied Report Filters".
- Do NOT change the run date, filter data, or anything else.
- Only edit this one file. Show both changes.

Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Bug #182 — two table fixes:

CHANGE 1 (center CAS PD / CAS LGD data): In the scorecard table (page 2), the 
data values for Bank PD and Bank LGD are already centered, but CAS PD and CAS 
LGD data are LEFT-aligned. Center them to match. (The headers are already 
centered — only fix the DATA cell alignment for CAS PD and CAS LGD.)
Find the CAS PD and CAS LGD DATA cells (the row cells, not headers) and add/
change textAlign to "center" so all four columns' data is centered.

CHANGE 2 (Scorecard ID wrap): The Scorecard ID column (~22% width) doesn't wrap 
long values — they overflow/cut off. Apply the same wrap fix we used in the 
memos: add a wrapAnywhere style (wordBreak: "breakAll") to the Scorecard ID 
cell so long IDs wrap within the column.
- If a wrapAnywhere style already exists in this file, apply it to the 
  Scorecard ID cell.
- If not, add: wrapAnywhere: { wordBreak: "breakAll" } to the stylesheet, and 
  apply styles.wrapAnywhere to the Scorecard ID data cell.
- Also, if the Scorecard ID uses any hyphen-zero-width-space injection 
  (like "-\u200b"), remove that injection and rely on wrapAnywhere (same as the 
  memo fix).
Keep the column width (~22%) unchanged — the ID should wrap within the existing 
width, not widen the table.

CONSTRAINTS:
- CHANGE 1: only add textAlign "center" to CAS PD and CAS LGD DATA cells (Bank 
  PD/LGD already centered).
- CHANGE 2: add wrapAnywhere (wordBreak breakAll) to the Scorecard ID cell; 
  remove any hyphen zero-width-space injection on it; keep the width.
- Do NOT change column widths, headers, other columns, or data values.
- Only edit this one file. Show the CAS PD/LGD data cells (centered) and the 
  Scorecard ID cell (wrapAnywhere).


READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmSummaryPDF.tsx

Concern: the report is now showing ALL customer names / customer IDs / review 
IDs for a single Sample ID combined together. Need to confirm whether the 
recent edits (header fontSize 20->14, filter heading rename, CAS PD/LGD 
textAlign center, Scorecard ID wrapAnywhere) affected data grouping/rendering.

Show:
1. Confirm the recent changes were ONLY: headerTitle fontSize, heading text 
   "Applied Report Filters", textAlign on CAS PD/LGD data cells, and 
   wrapAnywhere on Scorecard ID. Confirm NONE of these touched the data source, 
   the .map that iterates rows, any grouping/filtering logic, or the data 
   query.
2. Show how the report iterates its data — the rows.map (or items.map) that 
   renders customer name / customer ID / review ID. Is there any grouping by 
   Sample ID? Show the data structure being iterated.
3. Is this report DESIGNED to show all customers/reviews for a sample (a 
   sample-level summary), or one customer per row? Show what the data 
   represents.
4. Confirm the data comes from props (props.data / props.items) — i.e. the 
   backend provides the data, and this PDF file only renders it (doesn't 
   query/group). So if wrong data appears, it's from the data source, not these 
   formatting edits.

Do NOT edit. Confirm the recent edits didn't touch data logic, and show how 
data is iterated + whether combining-by-sample is by design or from the data 
source. Findings only.

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

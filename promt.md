READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

minPresenceAhead={40} on the heading Text did not fix the orphan. I want to wrap the heading + table together for the BOUNDED distribution sections. Show me ONLY (no edits):

1. Subreport04_DistByExposure — the COMPLETE JSX from its outer <View> through the heading and the entire table to the closing </View>. I need to see the exact wrapping structure so I can wrap heading+table in a single <View wrap={false}> (this table is bounded ~16 rows, so wrap={false} is safe).

2. Subreport03_DistByCount — same complete JSX.

3. Confirm: for these two bounded tables, is the outer <View> already a wrapper containing BOTH heading and table? If yes, I can just add wrap={false} to that existing outer View. Show me that outer View's current props.

4. What React-PDF version is in use (check package.json import or a comment)? Some versions handle minPresenceAhead on Text vs View differently.

Read once. Findings only. No edits.

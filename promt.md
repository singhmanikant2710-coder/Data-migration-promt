READ-ONLY. Diagnostics only. Do not change anything.

I need to make 3 changes to the CRO Review Production PDF report (similar to 
edits already done on the CRM PD Grade Migration report). First locate 
everything. Report back (no edits):

1. FIND THE COMPONENT: Locate the CRO Review Production PDF component (likely 
   frontend/src/components/pdf/CroReviewProductionPDF.tsx or similar). 
   Confirm the file path.

2. HEADER: Show the header code that renders the title "CRO Review Production" 
   and the top-right meta text (currently showing a sample number like "356"). 
   I need to change the meta to show ONLY the run date in MM/DD/YYYY format 
   (no time, no sample name/number), and ensure the meta font color is White. 
   Show the current header JSX and the meta text/color styles.

3. REMOVE COLUMN: In the per-reviewer detail table (columns: CUSTOMER NAME 
   (REVIEW ID), CUSTOMER #, UNIT, COMMITMENT, OUTSTANDING, BANK PD, CAS PD, 
   and a final "(STATUS NOT SELECTED)" column showing dates) — locate the 
   "(STATUS NOT SELECTED)" / status column. Show its header <Text> and its 
   body cell rendering. Confirm whether this header/cell appears in multiple 
   places (repeated per reviewer, or a shared header/row component). I need 
   to remove this column entirely.

4. FOOTER: Show the current footer code and all the places it's repeated. 
   I need to reformat it to mirror the CRM Summary footer: remove any logo, 
   centered single line reading "<Report Name> • Page X of Y". Confirm how 
   the report name/title is available in scope for the footer.

5. For the column removal: confirm the header column count and body cell 
   count so that after removing the status column, header and body still 
   match.

Do not edit anything. Findings only.

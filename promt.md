READ-ONLY first, then targeted revert. 

File: frontend/src/components/pdf/CrmSummaryPDF.tsx

The last edit (wordBreak "break-all", overflow "hidden" on table, lineHeight on 
PD/LGD) caused the page width to increase and headers to break/misalign. Need 
to isolate and fix.

STEP 1 (read-only): Show the current state of the three changes:
1. styles.table — does it now have overflow: "hidden"? Show the full 
   styles.table.
2. styles.wrapAnywhere — is it now wordBreak: "break-all"?
3. The PD/LGD Text cells — do they now have lineHeight: 1.2?
Also show: the header row structure and its column widths vs the data row 
column widths — did anything change the widths, or is a style causing columns 
to render wider?

STEP 2 (targeted revert to isolate): Revert ONLY overflow: "hidden" from 
styles.table (remove that one line), keeping wordBreak: "break-all" and the 
lineHeight changes. 
Reason: overflow: "hidden" on a flex table in react-pdf can force the container 
to size to content and break the layout/width. Removing it is the most likely 
fix. Keep the other two changes (break-all for wrapping, lineHeight for the K 
overlap) since those are correct and low-risk.

After removing overflow: "hidden", show styles.table.

CONSTRAINTS:
- Remove ONLY overflow: "hidden" from styles.table.
- Keep wordBreak: "break-all" (wrapAnywhere) and lineHeight: 1.2 (PD/LGD).
- Do NOT change column widths or anything else.
- Show styles.table after the revert.

READ-ONLY. Diagnostics only. Do not change anything.

After removing wrap={false} from the matrix ROOT (which fixed blank pages), 
Sample 354/374 now show WHITESPACE: when the next row (e.g. Totals/Changes/
%Change) doesn't fit at the bottom of a page, wrap={false} on that row pushes 
the WHOLE row to the next page, leaving whitespace on the previous page.

Confirm the current state:
1. Show the current MatrixCount and MatrixCommitment root and row structure — 
   confirm the root is now plain <View> (no wrap) and which inner elements 
   still have wrap={false} (header row, data rows, Totals, Changes, %Change).

2. Specifically: do the Totals / Changes / % Change rows have wrap={false} 
   individually? These are the ones likely being pushed to the next page.

3. Is there any grouping like <View wrap={false}> around the LAST THREE rows 
   (Totals + Changes + % Change) that forces them to stay together as a block, 
   which would push all three to the next page if they don't fit together?

4. Confirm the table container (styles.table) and whether it has any 
   wrap-related or minPresenceAhead property.

5. For a data row and for the Totals/Changes/%Change rows: is wrap={false} 
   actually NEEDED on individual single-line rows? A single-line row won't 
   split anyway if it fits; wrap={false} only matters if a row could span 
   multiple lines. Confirm whether these rows are single-line (so wrap={false} 
   on them is unnecessary and can be safely removed to eliminate the push-to-
   next-page whitespace).

Do not edit anything. Confirm which rows have wrap={false} and whether the 
last-three-rows are grouped. Findings only.

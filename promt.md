READ-ONLY. Diagnostics only. Do not change anything.

Investigating blank-page/pagination bug in CrmPdGradeMigrationPDF.tsx that 
appears for Sample 354 (large matrix) but not 356 (small matrix), after we 
reordered with <View break> wrappers around MatrixCommitment and DistCharts.

Show me:
1. The EXACT current JSX in CrmPdGradeMigrationPage from the header down 
   through the subreports (verbatim), including the <View break> wrappers.

2. MatrixCount and MatrixCommitment's OUTER element — confirm each returns 
   <View wrap={false}> as its root. So the structure is:
   <View break>              <- our reorder wrapper
     <View wrap={false}>     <- matrix's own root (from the component)
       ...matrix...
     </View>
   </View>
   Confirm this double-nesting (break wrapper containing a wrap={false} root).

3. The approximate height of MatrixCount for a large dataset: with the new 
   Changes + % Change rows, how many total rows does the matrix have (data 
   rows + Totals + Changes + % Change)? For a large sample like 354, could 
   MatrixCount alone exceed one page height? (Page content height ~484pt; 
   each row ~24pt; header ~44pt.) If data rows > ~16, the matrix + 3 total 
   rows + labels could approach or exceed one page — and with wrap={false} 
   it cannot split, forcing an overflow.

4. Whether a <View break> that wraps a wrap={false} block that OVERFLOWS a 
   page causes React-PDF to emit a blank page (the known interaction: break 
   forces a new page, then the un-splittable oversized block overflows, and 
   the fixed footer + break logic produce an empty page + missing footer).

5. styles.page paddingBottom and the footer's fixed positioning — confirm 
   the footer is <View style={styles.footer} fixed> as a direct child of 
   <Page>, and whether an overflowing wrap={false} block on page 1 would 
   suppress that page's fixed footer.

Do not edit anything. Confirm the double-nesting (break + wrap=false), the 
matrix height for large data, and the overflow interaction. Findings only.

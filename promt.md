READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

I'm implementing a generic "keep-together" pagination for the matrix bottom. 
Need the EXACT current structure of both matrices' data rows, Totals row, and 
footnote. Show for BOTH MatrixCount and MatrixCommitment:

1. The data rows render block (the rowsWithMetrics.map) — its current wrapper 
   and any wrap props.
2. The bottom Totals row — its EXACT current wrapper (plain <View> or 
   <View wrap={false}>?) and the full row.
3. The footnote — its EXACT current placement: inside <View style={table}>, 
   or sibling after the table? Show the closing </View> of the table and 
   where the footnote sits relative to it.
4. The matrix root View structure: 
   <View>  (root)
     <Text sectionTitle>
     <View style={table}>  ...header, data rows, Totals?...  </View>
     <footnote?>
   </View>
   Show this skeleton exactly — specifically whether the Totals row is INSIDE 
   styles.table or after it, and whether the footnote is inside or outside 
   styles.table.
5. Confirm styles.table has overflow:"hidden" and borderWidth (so I know if 
   moving rows outside it breaks the visual border).

Do not edit anything. Show the exact skeleton (root -> table -> rows -> Totals 
-> footnote) with every wrap prop and the precise inside/outside-table 
placement, for both matrices. Findings only.

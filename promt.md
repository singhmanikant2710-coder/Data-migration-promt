READ-ONLY. Diagnostics only. Do not change anything.

On page 1 of the CRM PD Grade Migration report, there is a large whitespace 
gap below the two PD Distribution charts (before the footer). The next 
section (PD Grade Migration matrix) starts on page 2. Geoff wants this 
whitespace reduced.

In CrmPdGradeMigrationPDF.tsx, show me:
1. The JSX from the DistCharts (the two bar charts) down through the next 
   element(s) — MatrixCount and MatrixCommitment. Show all the spacers 
   (<View style={styles.spacer} />), margins, and any wrap={false} or break 
   props between the charts and the matrix.
2. The height/margin of styles.spacer and any marginTop/marginBottom on the 
   chart container and the matrix container.
3. Whether MatrixCount has wrap={false} on its outer View (which would force 
   the whole matrix to move to the next page if it doesn't fit on page 1, 
   creating the gap).
4. The approximate height of the DistCharts block and MatrixCount block — 
   would MatrixCount realistically fit on page 1 below the charts, or is it 
   too tall (making the page-2 move unavoidable)?
5. Confirm: is the gap caused by (a) excessive spacer/margin, or (b) 
   MatrixCount being wrap={false} and too tall to fit, so it moves to page 2 
   and leaves the remainder of page 1 empty?

Do not edit anything. Findings only.

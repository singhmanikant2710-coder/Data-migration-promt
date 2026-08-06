Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

FIX (Sample 354 blank-page issue): The <View break /> markers before 
MatrixCommitment and before DistCharts force a new page even when the previous 
matrix has overflowed, leaving a blank page. Remove the forced page breaks so 
sections flow naturally, minimizing whitespace (per client's note that some 
whitespace is acceptable, less than the alternative).

Remove the two <View break /> markers:
- The <View break /> between MatrixCount and MatrixCommitment — REMOVE it.
- The <View break /> between MatrixCommitment and DistCharts — REMOVE it.

So the sequence becomes:
    <MatrixCount ... />
    <MatrixCommitment ... />
    <DistCharts ... />
    <View style={styles.spacer} />
    <Subreport01_Count ... />
    ...

The matrices and charts now flow naturally: when a matrix is tall (large 
sample), the next section continues on the same page if there's room, or 
flows to the next page without forcing a blank page. Small samples (356) may 
now fit both matrices closer together — this is fine and more space-efficient.

CONSTRAINTS:
- Only remove the two <View break /> markers. Do NOT change anything else.
- Keep the matrix content, header/footer notes, Changes/%Change rows, colors, 
  and the section ORDER (MatrixCount -> MatrixCommitment -> DistCharts -> 
  subreports).
- Keep the row wrap settings as-is (header wrap={false}, data/summary rows 
  no wrap).
- Do NOT touch pageSetup.ts, page size, margins, the page footer, or backend.
- Only edit this one file. Show the sequence with the breaks removed.

Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

PROBLEM: After removing the Selection Summary section, a large whitespace 
gap appears on page 1. The "PD Grade Migration by Scorecard Count" heading 
renders at the bottom of page 1, but its table's header row has 
minPresenceAhead: 28 (in styles.trHeader), which pushes the whole table to 
the next page — stranding the heading with empty space beneath it.

GOAL: Keep each matrix's heading together with its table so the heading is 
never stranded. When there isn't enough room, the heading AND table should 
move to the next page together.

In the MatrixCount component, wrap the heading <Text> and the table <View> 
together in a single grouping View with wrap={false}, so they stay on the 
same page as a unit.

Current structure (inside MatrixCount return):
    return (
      <View>
        <Text style={styles.sectionTitle}>PD Grade Migration by Scorecard Count</Text>
        <View style={styles.table}>
          ...header row, data rows, totals...
        </View>
      </View>
    );

Change the OUTER <View> to keep the heading with the table. The cleanest 
approach: wrap the heading + table in a <View wrap={false}> so react-pdf 
moves them together:
    return (
      <View wrap={false}>
        <Text style={styles.sectionTitle}>PD Grade Migration by Scorecard Count</Text>
        <View style={styles.table}>
          ...unchanged...
        </View>
      </View>
    );

IMPORTANT CHECK: If the table can ever be tall enough that heading+table 
together exceed one full page, wrap={false} could cause overflow/clipping. 
Given the matrix omits empty rows (max ~16 rows) and fits within one page, 
wrap={false} is safe here. If MatrixCommitment has the same heading-strand 
pattern and the same minPresenceAhead issue, apply the SAME wrap={false} 
grouping to its outer View too, for consistency.

CONSTRAINTS:
- Do NOT change the table contents, cell logic, colors, totals, or 
  minPresenceAhead value itself.
- Do NOT touch DistCharts, the header, or any other section.
- Only edit this one file. Show the changed lines for MatrixCount (and 
  MatrixCommitment if you apply the same grouping).

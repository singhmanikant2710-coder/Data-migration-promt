Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

GOAL (Geoff comment #5): The report labels say "Scorecard Count", but the 
underlying data already counts Number of Accounts (the repository joins 
dbo.[02_CORE_04_Accounts] and counts account rows; there is no scorecards 
table). Geoff wants the report to reflect "Number of Accounts". This is a 
LABEL-ONLY change — the data/query is already correct and must NOT change.

Update the visible text in the following places, replacing "Scorecard Count" 
with "Number of Accounts":

1. The PD distribution chart title:
   Current: <PdBarChart title="PD Distribution by Scorecard Count" ... />
   Change to: title="PD Distribution by Number of Accounts"

2. The matrix section heading in MatrixCount:
   Current: <Text style={styles.sectionTitle}>PD Grade Migration by Scorecard Count</Text>
   Change to: PD Grade Migration by Number of Accounts

Search the file for any other occurrence of the literal "Scorecard Count" 
tied to this count-based chart/table and update it the same way. 

DO NOT change:
- "Scorecard Commitment" labels (the commitment/dollar charts are a 
  different, correct label — leave them exactly as-is).
- Any query, aggregation, data source, count logic, cell values, or totals.
- The "by Scorecard Commitment" chart/matrix.

Only edit this one file. List every text change made and confirm no 
"Scorecard Commitment" labels or data logic were touched.

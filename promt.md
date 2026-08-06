READ-ONLY. Diagnostics only. Do not change anything.

File: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

Three formatting requests for the PD Grade Migration report. Investigate 
current state before editing:

ITEM 5 — "0" in empty Totals/Changes cells (Accounts/count matrix):
In MatrixCount, show how the Totals row and Changes row cells render empty/
zero values. Currently zero cells appear blank. I need them to show "0". 
Show the exact cell render for the Totals row and Changes row (how a 0 is 
displayed now).

ITEM 6 — "$0" in empty cells (Commitment matrix):
In MatrixCommitment, show how empty/zero cells render. Client wants empty 
cells to show "$0" with the SAME color schema as the Accounts matrix (the 
pink/green directional coloring). Show current empty-cell handling and the 
color logic (bg/fg based on toPd vs fromPd) so I can apply "$0" with matching 
colors.

ITEM 7 — SM/SUB/DFUL/LOSS monikers in DETAIL table:
The blue-banner Detail table shows PD values (BANK PD, CAS PD columns). 
Client wants SM/SUB/DFUL/LOSS monikers added to the relevant PDs (13=SM, 
14=SUB, 15=DFUL, 16=LOSS) in the detail table, like the matrix column labels. 
Show how the Detail table (DetailTableRows / DetailTablePage) renders the 
BANK PD and CAS PD values, so I can add the moniker suffix (e.g. "13 / SM") 
to PD 13-16.

Do not edit anything. Show the Totals/Changes cell render (MatrixCount), the 
empty-cell + color logic (MatrixCommitment), and the Detail table PD cell 
render. Findings only.

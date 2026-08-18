Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Fix (Item 6): Reduce the Scorecard Assessment table header height by lowering vertical padding on ONLY its header cells (shared styles.th has padding: 8). Add an inline paddingVertical: 4 override to each Scorecard Assessment header cell. Do NOT change styles.th (that would affect Policy Exception and Unsatisfactory tables).

Add { paddingVertical: 4 } to the style array of EACH Scorecard Assessment header cell (sc1 through sc8):

BEFORE (example, SCORECARD ID):
<View style={[styles.th, styles.sc1]}><Text style={styles.thText}>SCORECARD ID</Text></View>
AFTER:
<View style={[styles.th, styles.sc1, { paddingVertical: 4 }]}><Text style={styles.thText}>SCORECARD ID</Text></View>

Apply the same { paddingVertical: 4 } addition to ALL eight header cells:
- sc1 (SCORECARD ID)
- sc2 (DATE)
- sc3 (BANK PD)
- sc4 (BANK LGD)
- sc5 (CAS PD)
- sc6 (CAS LGD)
- sc7 (SCORECARD TYPE)
- sc8 (SCORECARD ASSESSMENT) — keep its existing styles.tdLast too: [styles.th, styles.sc8, styles.tdLast, { paddingVertical: 4 }]

CONSTRAINTS:
- ONLY add { paddingVertical: 4 } to the eight Scorecard Assessment HEADER cells.
- Do NOT modify styles.th, styles.thText, or any other table's headers (Policy Exception, Unsatisfactory).
- Do NOT change the value/data row cells, only the header row.
- Do NOT change widths or any prior fix.
- Show the FULL diff.

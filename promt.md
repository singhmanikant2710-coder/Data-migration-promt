Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Several refinements to the Scorecard Assessment table (Geoff #182 follow-up). Total column width must stay exactly 100%.

=== CHANGE 1 — "CAS PD" header to two lines (match BANK PD/LGD, CAS LGD) ===
BEFORE:
<Text style={styles.thText}>CAS PD</Text>
AFTER:
<Text style={styles.thText}>CAS{"\n"}PD</Text>

(Also confirm BANK PD, BANK LGD, CAS LGD already render two-line; if BANK PD is still one line, apply same: "BANK{"\n"}PD" etc. But per screenshot only CAS PD needs it. Only change CAS PD unless BANK PD is also single-line.)

=== CHANGE 2 — Center CAS PD and CAS LGD VALUES (mirror BANK PD/LGD) ===
BEFORE (CAS PD value):
<View style={[styles.td, styles.sc5]}><Text style={styles.tdText}>{out((row as any)?.casPd)}</Text></View>
AFTER:
<View style={[styles.td, styles.sc5]}><Text style={[styles.tdText, { textAlign: "center", width: "100%" }]}>{out((row as any)?.casPd)}</Text></View>

BEFORE (CAS LGD value):
<View style={[styles.td, styles.sc6]}><Text style={styles.tdText}>{out((row as any)?.casLgd)}</Text></View>
AFTER:
<View style={[styles.td, styles.sc6]}><Text style={[styles.tdText, { textAlign: "center", width: "100%" }]}>{out((row as any)?.casLgd)}</Text></View>

=== CHANGE 3 — Rebalance widths: reduce PD/LGD, add to Scorecard ID + Assessment ===
Total stays 100%.
BEFORE -> AFTER:
sc1: 21.5% -> 24%     (Scorecard ID, more space)
sc3: 7% -> 6%         (Bank PD)
sc4: 7% -> 6%         (Bank LGD)
sc5: 8% -> 6%         (CAS PD)
sc6: 7% -> 6%         (CAS LGD)
sc8: 21.5% -> 24%     (Scorecard Assessment, more space)
(sc2=10%, sc7=18% unchanged. New total: 24+10+6+6+6+6+18+24 = 100%.)

For each, update BOTH flexBasis and width to the new percentage.

CONSTRAINTS:
- Keep total width exactly 100%.
- Do NOT change the shared th/thText style definitions (only per-cell/value changes and width classes).
- Do NOT change sc2, sc7.
- Do NOT touch other tables or files.
- Show the FULL diff.

Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Fix: "BANK PD" / "BANK LGD" / "CAS PD" / "CAS LGD" header text overflows the column border because sc3–sc6 are only 5% wide. Widen these four columns from 5% to 7% each (+8% total), and reduce sc8 from 30% to 22% (-8%) to keep the table total at exactly 100%.

Exact changes (before -> after):

sc3: { flexGrow: 0, flexShrink: 0, flexBasis: "5%", width: "5%" }
  -> sc3: { flexGrow: 0, flexShrink: 0, flexBasis: "7%", width: "7%" }

sc4: { flexGrow: 0, flexShrink: 0, flexBasis: "5%", width: "5%" }
  -> sc4: { flexGrow: 0, flexShrink: 0, flexBasis: "7%", width: "7%" }

sc5: { flexGrow: 0, flexShrink: 0, flexBasis: "5%", width: "5%" }
  -> sc5: { flexGrow: 0, flexShrink: 0, flexBasis: "7%", width: "7%" }

sc6: { flexGrow: 0, flexShrink: 0, flexBasis: "5%", width: "5%" }
  -> sc6: { flexGrow: 0, flexShrink: 0, flexBasis: "7%", width: "7%" }

sc8: { flexGrow: 0, flexShrink: 0, flexBasis: "30%", width: "30%" }
  -> sc8: { flexGrow: 0, flexShrink: 0, flexBasis: "22%", width: "22%" }

CONSTRAINTS:
- ONLY change the flexBasis and width values for sc3, sc4, sc5, sc6, sc8.
- Do NOT touch sc1, sc2, sc7, or any other style.
- Do NOT change th, thText, td, tdText, or any header/value logic.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

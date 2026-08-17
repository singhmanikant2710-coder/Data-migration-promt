Single-file edit: frontend/src/components/pdf/CrmSummaryPDF.tsx

Fix: "CAS PD" (sc5) renders on a single line while the other three grade columns wrap to two lines, looking inconsistent. Widen sc5 slightly (7% -> 8%) so "CAS PD" has room to sit uniformly, and reclaim that 1% by shaving 0.5% each from sc1 and sc8 (both have spare room). Table total stays exactly 100%.

Exact changes (before -> after):

sc5: { flexGrow: 0, flexShrink: 0, flexBasis: "7%", width: "7%" }
  -> sc5: { flexGrow: 0, flexShrink: 0, flexBasis: "8%", width: "8%" }

sc1: { flexGrow: 0, flexShrink: 0, flexBasis: "22%", width: "22%" }
  -> sc1: { flexGrow: 0, flexShrink: 0, flexBasis: "21.5%", width: "21.5%" }

sc8: { flexGrow: 0, flexShrink: 0, flexBasis: "22%", width: "22%" }
  -> sc8: { flexGrow: 0, flexShrink: 0, flexBasis: "21.5%", width: "21.5%" }

CONSTRAINTS:
- ONLY change flexBasis and width for sc5, sc1, sc8.
- Do NOT touch sc2, sc3, sc4, sc6, sc7.
- Do NOT change th, thText, td, tdText, or the scorecardId render line (Fix 2 stays).
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

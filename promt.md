Two-file edit: 
- frontend/src/components/pdf/InitialMemoPDF.tsx
- frontend/src/components/pdf/FinalMemoPDF.tsx

GOAL (Geoff): In the "Informational Purposes" table, reduce the Code column 
width and add that space to the Comments column, mirroring the "Credit Risk 
Management Findings" table widths. Apply to BOTH the Initial Memo and Final 
Memo reports.

Reference (Credit Risk Management Findings, cols3Findings): 
  Code 10% | Type 30% | Comments 60%

The Informational Purposes table uses the `cols3Observations` StyleSheet 
constant, currently:
  c1 (Code): flexBasis "20%"
  c2 (Observation Type): flexBasis "30%"
  c3 (Comments): flexBasis "50%"

Change `cols3Observations` in BOTH files to mirror the findings widths:
  c1 (Code): flexBasis "20%"  ->  flexBasis "10%"
  c2 (Observation Type): flexBasis "30%"  (unchanged)
  c3 (Comments): flexBasis "50%"  ->  flexBasis "60%"

Apply this EXACT change to cols3Observations in:
1. InitialMemoPDF.tsx
2. FinalMemoPDF.tsx

CONSTRAINTS:
- Change ONLY the flexBasis values of cols3Observations.c1 (20% -> 10%) and 
  cols3Observations.c3 (50% -> 60%). Leave c2 at 30%.
- Do NOT touch cols3Findings, the header text, the table structure, or any 
  other style/table.
- The three widths must still sum to 100% (10 + 30 + 60 = 100). ✓
- Do NOT change pageSetup.ts, page size, orientation, margins, or any other 
  component.
- Only edit these two files. Show the updated cols3Observations constant 
  from each file.

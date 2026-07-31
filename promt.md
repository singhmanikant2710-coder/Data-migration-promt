Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

GOAL (Geoff comment #3): The header meta text (the run date on the right 
side of the header bar) currently renders in light blue and looks faded. It 
should be solid White to match the "CRM PD Grade Migration" title.

In the styles object, the headerMeta style currently has:
    color: "#BFDBFE"

Change it to:
    color: "#FFFFFF"

CONSTRAINTS:
- Change ONLY the color value on headerMeta. Do NOT change fontSize, 
  textAlign, or any other property.
- Do NOT touch headerTitle or any other style.
- Only edit this one file. Show the changed line.

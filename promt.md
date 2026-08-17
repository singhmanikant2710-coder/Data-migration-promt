Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix: Header vs row vertical gridlines don't line up because header cells and data cells use different padding: thDark padding 6, thBlue padding 2, td padding 4. Align the header padding to match the row padding (4) so the vertical borders line up.

BEFORE (thDark): padding: 6
AFTER  (thDark): padding: 4

BEFORE (thBlue): padding: 2
AFTER  (thBlue): padding: 4

CONSTRAINTS:
- ONLY change the `padding` value in thDark (6 -> 4) and thBlue (2 -> 4).
- Do NOT change borders, fontSize, flexGrow, lineHeight, or anything else.
- Do NOT change td or any row style.
- Do NOT touch column widths (f*/c*) or any other file.
- Only edit this one file. Show the diff.

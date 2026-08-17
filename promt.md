Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix: Table column header font size should be 9 (currently 10). The header text uses thDark and thBlue styles directly. Change fontSize 10 -> 9 in BOTH.

BEFORE (thDark): fontSize: 10   (within the thDark style object)
AFTER  (thDark): fontSize: 9

BEFORE (thBlue): fontSize: 10   (within the thBlue style object)
AFTER  (thBlue): fontSize: 9

CONSTRAINTS:
- ONLY change fontSize from 10 to 9 in thDark and thBlue.
- Do NOT change padding, borders, flexGrow, lineHeight, or anything else in those styles.
- Do NOT change td (row) font size or any other style.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

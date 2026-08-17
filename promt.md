Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix: In the six-column detailed table, the SEVERITY column (f3) is too narrow to fit "Observation", and COMMENTS (f6) is the widest. Slightly increase SEVERITY's flex weight and reduce COMMENTS' by the same amount, so total proportion is preserved.

BEFORE:
f3: { flexGrow: 1, flexShrink: 1, flexBasis: 0 }
...
f6: { flexGrow: 8, flexShrink: 1, flexBasis: 0 }

AFTER:
f3: { flexGrow: 2, flexShrink: 1, flexBasis: 0 }
...
f6: { flexGrow: 7, flexShrink: 1, flexBasis: 0 }

CONSTRAINTS:
- ONLY change flexGrow for f3 (1 -> 2) and f6 (8 -> 7).
- Do NOT change f1, f2, f4, f5.
- Do NOT change flexShrink or flexBasis on any column.
- Do NOT touch the consolidated table (c1-c3) or any shared style.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

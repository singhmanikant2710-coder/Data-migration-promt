Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix (Item 1a follow-up): Geoff wants the header DATE font to be 9 (mirroring CRM Summary, where the date is fontSize 9). Currently headerMeta (the date) is 14. Change ONLY headerMeta fontSize to 9. Leave headerTitle at 14.

BEFORE (headerMeta style): fontSize: 14
AFTER  (headerMeta style): fontSize: 9

CONSTRAINTS:
- ONLY change fontSize 14 -> 9 in the headerMeta style object.
- Do NOT change headerTitle (stays 14).
- Do NOT change color, textAlign, or anything else in headerMeta.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

Single-file edit: frontend/src/components/pdf/CrmFindingsObservationsPDF.tsx

Fix (Item 2 follow-up): "Observation" still runs into the border. Widen SEVERITY (f3) by one more step and reduce COMMENTS (f6) to compensate.

BEFORE:
f3: { flexGrow: 2, flexShrink: 1, flexBasis: 0 }   // SEVERITY
f6: { flexGrow: 7, flexShrink: 1, flexBasis: 0 }   // COMMENTS
AFTER:
f3: { flexGrow: 3, flexShrink: 1, flexBasis: 0 }   // SEVERITY
f6: { flexGrow: 6, flexShrink: 1, flexBasis: 0 }   // COMMENTS

CONSTRAINTS:
- ONLY change flexGrow for f3 (2->3) and f6 (7->6).
- Do NOT change f1, f2, f4, f5, or flexShrink/flexBasis.
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

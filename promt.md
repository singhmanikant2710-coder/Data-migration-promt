Bug 207 fix — CAS Linesheet PDF drops "No Monitoring/Performance Covenants" rows because its category filters don't admit the "No Covenants" category. Extend the two filters to include it, gated on covenantType, mirroring useCovenants.ts exactly. Frontend-only, ONE file. Show diff before applying.

FILE: frontend/src/components/pdf/ReviewPDF.tsx (lines ~705-717)

The "No Covenants" row has category "No Covenants" and a covenantType of either "No Monitoring Covenants" or "No Performance Covenants". Currently monitoringRows and financialRows filters admit neither, so the row is discarded.

Extend the filters to mirror useCovenants.ts (lines ~116-121):
- monitoringRows: keep existing categories, AND also include a row when normalized category === "no covenants" AND its covenantType is NOT "no performance covenants".
- financialRows: keep existing categories, AND also include a row when normalized category === "no covenants" AND its covenantType is NOT "no monitoring covenants".

Use the same covenantType field resolution the row already uses (mirror the ?? fallback chain used for category, applied to covenantType). Normalize with trim().toLowerCase() consistently.

The existing render maps (lines ~1120-1127 and ~1143-1152) work unchanged — the row will print covenantType ("No Monitoring Covenants" / "No Performance Covenants") in the first column with remaining cells blank (those DB fields are null for these rows). Do NOT add a new render branch. Do NOT use a placeholder-when-empty approach.

This is purely additive (widens two filters); no currently-rendered row can be dropped or moved. Do NOT touch backend, DTO, the Covenant Comments section, covAccTrack/covAccValid/covBreaches, or anything else.
List every line changed. Commit: "Fix Bug 207: CAS Linesheet PDF shows No Monitoring/Performance Covenants rows (extend covenant category filters)".

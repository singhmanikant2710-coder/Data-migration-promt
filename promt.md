READ-ONLY. No code changes.

Goal: when a user edits a field on the Blackbook edit page, the live
(pre-save) values must match legacy. Today some dependent values show
wrong numbers until Refresh/Save, then become correct.

Produce, per industry (13), a compact inventory:

1. EDITABLE FIELDS: every input the user can edit on the edit page (Top
   Strip, Monthly Summary inline, Month/TTM, Cash & Charge-offs, right
   rail, covenants, custom fields). Field label -> tblMain column.
2. DEPENDENTS: for each editable field, which displayed fields depend on
   it (directly or via other calculated fields).
3. FRONTEND LIVE PREVIEW: for each dependent, what the frontend computes
   before save (file:line + formula), or "not recomputed".
4. BACKEND ON SAVE: which method recomputes it (SqlMainRepository /
   TblMainCalcs / TTM / YTD paths) + formula, file:line.
5. LEGACY: is it an Access CALCULATED FIELD on tblMain (same-row,
   updates live on edit — quote expression) or computed in funSave /
   queries on Save (cross-row: YTD, TTM, prior month — quote)?
6. MISMATCH: dependents where frontend preview formula != legacy
   formula, or where the frontend estimates a cross-row value (YTD/TTM)
   that legacy only updates on Save.

Classify every dependent:
 A = same-row legacy calculated field -> should update LIVE on frontend
 B = cross-row (YTD/TTM/prior month) -> should keep the stored value
     until Save (legacy behaviour)

Output: one table per industry (field | column | dependents | frontend
preview | backend | legacy class A/B | mismatch yes/no), then a short
list of all mismatches. Report only.

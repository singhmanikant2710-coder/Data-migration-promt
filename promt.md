Read-only diagnostic, no edits.

Question about a possible regression from the FormChangesContext mergeValues fix.

Does the Covenants section (CovenantsSection.tsx) stage ARRAYS into FormChangesContext via setSection/setField? Show me every setSection/setField call in that file and the shape of the payload.

Context: the OLD code did `{ ...prevEntry, ...next }` whenever both were `typeof === "object"` — and since `typeof [] === "object"`, any array staged there was already being corrupted into a plain object with numeric keys. The NEW mergeValues keeps arrays as arrays.

So tell me:
1. Does Covenants stage arrays? If YES, it was ALREADY broken before my change (arrays were being turned into objects), so the new fix cannot have caused a new regression — it can only have changed the shape of an already-broken behaviour.
2. If Covenants stages only plain objects/scalars, then mergeValues behaves IDENTICALLY to the old code for it — so it cannot be the cause either.
3. Either way, explain what actually happens on "Add covenant → Save" and why the newly added covenant does not appear in the UI after save. Trace the save response handling and the re-render path.

Report with evidence. STOP before editing.

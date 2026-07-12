UAT #57 — root cause found. Fresh start (I have reverted all previous #57 attempts).

ROOT CAUSE (proven at runtime via console log):
The pending snapshot in FormChangesContext holds the findings rows correctly — the data IS there:
  pendingSection = { findings: [ {id:"row-1",...}, {id:"row-2",...}, {id:"row-1783864659418", component:"01-Risk Recognition",...} ] }
BUT `Array.isArray(pendingSection.findings)` returns FALSE. The findings value is an array-LIKE OBJECT, not a real array. This is why any `Array.isArray(pending)` check fails and the UI falls back to saved data, making unsaved rows disappear.

This almost certainly comes from FormChangesContext's setSection doing an object spread/merge that converts the array into a plain object (e.g. `{ ...prev[key], ...payload }` where payload.findings is spread into an object, or `{ ...array }`).

YOUR TASK:
1. FIRST (read-only): show me the exact implementation of `setSection` (and any merge/reducer logic) in FormChangesContext. Identify precisely where the findings array loses its array type. Confirm the mechanism with evidence.
2. THEN propose the correct fix at the SOURCE — so that arrays stored in FormChangesContext stay real arrays (do not spread arrays into objects when merging). Do not patch around it in the consuming components with Object.values() hacks.
3. After that root fix, implement UAT #57 cleanly:
   a. CrmFindingsAndRatingsSection.tsx: stage a snapshot to FormChangesContext on Add Row and Delete Row (currently only field edits do).
   b. CrmFindingsAndRatingsSection.tsx: render the findings table from the pending snapshot when one exists, falling back to saved state.findings.
   c. useCrmFindings.ts + CrmRatingsSection.tsx: derive findingCounts from the effective findings (pending ?? saved) and wire the five StatCards to it (?? 0 fallback). Counts = rows where Severity === "Finding", components 01–05. DISPLAY-ONLY.

Constraints:
- Fixing setSection must NOT break other sections that use FormChangesContext (Customer Info, Covenants, etc.) — check what else relies on it and confirm the fix is safe for them.
- Do NOT change the save path, setRating, or the UNSAT checkboxes.
- No new API calls.

Report the setSection analysis and your plan FIRST. STOP for approval before editing.

Approved — implement the full fix. This requires 2-3 files. Do it in ORDER, pausing after each file for my confirmation.

STEP 1 — CrmFindingsAndRatingsSection.tsx: stage pending changes on Add Row and Delete Row too (currently only field edits stage a snapshot to FormChangesContext). After addEmptyRow/deleteRow, immediately call changes.setSection("crmFindingsAndRatings", { findings: <full updated array> }) using the same mapping as the field handlers.

STEP 2 — CrmFindingsAndRatingsSection.tsx (display fix): while in Edit mode, the findings TABLE must render from the pending FormChangesContext snapshot when one exists, falling back to saved state.findings otherwise. This makes unsaved rows survive a tab switch, consistent with how Customer Info already works. Do NOT change the save path — Save must continue to send exactly what it sends today.

STEP 3 — useCrmFindings.ts + CrmRatingsSection.tsx: derive findingCounts from the effective findings (pending snapshot ?? saved state.findings) and wire the five StatCards to findingCounts (?? 0 fallback). findingCounts stays DISPLAY-ONLY — never written to state, never in a save payload. effectiveFindings must be used ONLY for the count derivation, not to replace state.findings elsewhere in the hook.

Constraints across all steps:
- No new API calls.
- Do NOT touch setRating, the UNSAT checkboxes, or the save/persist logic.
- Cancel must still discard pending changes (FormChangesContext.clear()) and restore saved data.
- Resolve the correct import path for useFormChangesOptional against the actual file location.

Show me the diff for STEP 1 first. STOP after each step for my confirmation.

Approved — apply the 2-file change as planned:
1. useCrmFindings.ts: read the in-edit findings snapshot from FormChangesContext (useFormChangesOptional) and derive findingCounts from `effectiveFindings` (pending snapshot ?? saved state.findings).
2. CrmRatingsSection.tsx: destructure findingCounts and wire the five StatCard values (with ?? 0 fallback).

Conditions:
- findingCounts must remain DISPLAY-ONLY. Do not write counts to state.findings, do not include them in any save payload, and do not alter setRating, the UNSAT checkboxes, or the save path in any way.
- effectiveFindings must be used ONLY for the count derivation — it must NOT replace state.findings anywhere else in the hook (the CRM Findings table itself must keep rendering from its existing source, or we risk breaking editing on that tab).
- Verify the import path for useFormChangesOptional is correct (your draft showed two conflicting paths — resolve it against the actual file location, the same way other sections import it).
- After the change, confirm the CRM Findings tab still edits/adds/deletes rows normally (no regression from the hook change).

Show me the diff. STOP if any other file needs changing.

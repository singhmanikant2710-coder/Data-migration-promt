Approved — apply the single-file change to CrmRatingsSection.tsx exactly as planned:
- Destructure findingCounts from useCrmFindings.
- Wire the five StatCard values to findingCounts (with ?? 0 fallback).
- Do NOT touch the UNSAT checkboxes, setRating, or any save logic.

One check before you apply: confirm that `state.findings` in useCrmFindings reflects UNSAVED in-edit changes made on the CRM Findings tab (not just the last saved payload), so the counts update live as the user adds/edits/deletes findings without saving. If it only reflects saved data, tell me — do not work around it silently.

Show me the diff. STOP if another file needs changing.

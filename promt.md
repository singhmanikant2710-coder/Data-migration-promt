Approved. Proceed with the client-only approach — no backend changes needed since the findings/library endpoint already returns descriptions.

Step-by-step:
1. useCrmFindings.ts — add read-only loader that fetches findings/library?component={component} and builds labelByComponent[component][code] = `${code} - ${description}` (fallback to `${code}` if description null). Expose as FINDING_LABELS.
2. CrmFindingsAndRatingsSection.tsx — change option rendering to show the label, keeping value={code} unchanged.

Important:
- The option VALUE must stay the Finding_code only (do not change what gets saved).
- Fetch descriptions once per component (not on every render) — use a cache so switching rows/components doesn't refetch repeatedly. Confirm no fetch loops.

Pause after each file for my confirmation.

Read-only diagnostic, no edits.

In useCrmFindings.ts, show me the FULL label-building useEffect, specifically:
1. Its dependency array (exact deps).
2. Where `codes` comes from INSIDE the effect for each `comp` — is `codes` read from codeMap[comp] (FINDING_CODE_OPTIONS) at the moment the effect runs?
3. The guard logic that decides whether to skip a component (labelMap[comp] exists check and inFlight check) — does it skip a component whose labelMap[comp] was previously set to an EMPTY object {}?
4. The order of state updates: is it possible the effect runs BEFORE codeMap is populated (i.e. codes is empty []), builds an empty map, sets labelMap[comp] = {}, and then never re-runs because the guard sees labelMap[comp] already exists?

Report the deps array and whether an empty-codes early build can permanently cache an empty label map. STOP and ask before editing.

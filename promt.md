UAT #53 — CRM Findings Finding Code dropdown must show "Code - Description".

I'm attaching two screenshots:
1. CLIENT EXPECTED (Access prototype): the Finding Code dropdown lists each option as the code plus its description, e.g. "SS-101  Document imaging (or indexing) needs updating".
2. CURRENT APP: the dropdown lists only codes (SS-101, SS-102...). No descriptions.

WHAT WE ALREADY VERIFIED (do not re-verify, take as given):
- GET /api/v1/findings/library?component=... returns 200 with correct data. Response items are camelCase:
  { "component": "04-Credit Servicing", "findingCode": "CS-104", "description": "Covenant compliance monitoring lacks timeliness...", "category": "...", "guidance": "..." }
- useCrmFindings.ts fetches this via listCasFindingsLibrary (authenticated, from @/services/api/casFindings), builds labelMap[comp][code] = `${code} - ${description}`, and returns it as FINDING_LABELS.
- CrmFindingsAndRatingsSection.tsx destructures `FINDING_LABELS: labelOpts`, computes `const labelOptions = row.component ? (labelOpts[row.component] ?? {}) : {};` and renders `<option key={code} value={code}>{labelOptions[code] ?? code}</option>`.
- Names/keys match on both sides. Codes DO render (so codeMap is populated). But labels never appear — the render always falls back to `code`, meaning labelOpts[row.component] is empty/undefined at render time.

CONCLUSION: this is a timing / reactivity / state-propagation bug, not a naming or auth bug.

YOUR TASK:
Investigate end-to-end WHY labelMap never reaches the render (e.g. the label-building effect not firing, its deps, setLabelMap not triggering a re-render, memoization returning a stale reference, the component key used to write vs read, or codeMap resolving component names differently than row.component). Add temporary console logs if needed to prove the root cause.

Then propose ONE minimal fix. Requirements:
- Option value MUST stay the raw finding code (save path unchanged).
- Displayed label: "CODE - Description".
- Must work for ALL components (both CS-* and SS-* rows), not just the first one loaded.
- No fetch loops.

Report the ROOT CAUSE with evidence first, then show the proposed diff. STOP and wait for my approval before applying. Single file if possible; if two files are truly needed, say so and explain.

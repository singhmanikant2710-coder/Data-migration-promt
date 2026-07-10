Read-only diagnostic first, no edits.

In useCrmFindings.ts, show me the exact label-building code inside the effect that maps findings/library items to labels. I need to see which property names it reads from each item (e.g. item.Description vs item.description, item.Finding_code vs item.findingCode).

Context: the API response for /api/v1/findings/library returns items with these exact camelCase fields:
  { "component": "...", "findingCode": "CS-104", "description": "...", "category": "...", "guidance": "..." }

Confirm whether the current code reads the correct field names (findingCode, description). Report the mismatch. STOP and ask before editing.

Approved — single-file edit to useCrmFindings.ts only.

Fix the field-name mismatch in the label-building logic. The API returns camelCase fields: `findingCode` and `description`.

For each item, build:
  const code = item.findingCode;
  const desc = item.description;
  label = desc && desc.trim() ? `${code} - ${desc}` : code;

Keep the option value = code (findingCode). Keep the cache/in-flight guard unchanged. Do not touch save logic. STOP and ask if any other file needs changing.


Read-only diagnostic, no edits.

I need to see how the dropdown labels are RENDERED in CrmFindingsAndRatingsSection.tsx. The label-building in useCrmFindings.ts is correct and the API returns descriptions, but the dropdown still shows only codes.

Show me:
1. In CrmFindingsAndRatingsSection.tsx: the exact JSX where the Finding Code dropdown <option> (or SelectField options) is rendered — specifically how it computes each option's display label. Which variable/map does it read from (e.g. labelMap[component]?.[code], FINDING_LABELS, etc.)?
2. In useCrmFindings.ts: what the hook RETURNS (does it expose labelMap? a getLabel(component, code) helper? FINDING_LABELS?), and how `codes` is derived for each component (where does the `codes` array come from — is it populated at the time the effect runs?).
3. Confirm the `component` string used as the labelMap key on the render side EXACTLY matches the `comp` key used when writing to labelMap (e.g. "04-Credit Servicing" both sides — no trimming/casing diff).

Report findings only. STOP and ask before any edit.

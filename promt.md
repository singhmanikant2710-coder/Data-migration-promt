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

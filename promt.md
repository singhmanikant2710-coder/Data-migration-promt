Single-file edit: frontend/src/app/review/[ecif]/review-info/components/sections/CovenantsSection.tsx

Fix Bug #189 — Covenants screen.

ISSUE 1 — Color coding:
For "Is Stepped Up Servicing Required", Geoff expects:
- Yes = RED
- No = GREEN

Currently the color logic is reversed:
- Yes = GREEN
- No = RED

Change ONLY the existing color mapping:

BEFORE:
Yes → #047857 (green)
No  → #dc2626 (red)

AFTER:
Yes → #dc2626 (red)
No  → #047857 (green)

ISSUE 2 — N/A must remain N/A:
These three existing dropdowns already support "Yes", "No", and "N/A":

- accuratelyDefinedTracked
- accuratelyCalculated
- breachesMitigated

Currently selecting "N/A" and saving eventually results in "No".

The existing onChange code already passes the selected string through changes.setField(), so inspect this file for any existing logic that converts/defaults these values from "N/A" to "No".

If the conversion exists in this file, change ONLY that logic so:

Yes → Yes
No → No
N/A → N/A

Do NOT change the existing dropdown options or onChange behavior.

CONSTRAINTS:
- ONLY edit frontend/src/app/review/[ecif]/review-info/components/sections/CovenantsSection.tsx
- Do NOT touch any other file.
- Do NOT refactor or restructure the component.
- Do NOT change any unrelated Covenants fields.
- Do NOT change Save behavior except the specific N/A → No conversion if it exists in this file.
- Do NOT change API/backend/database code.
- Do NOT change PDF code.
- Do NOT modify any other tab or existing functionality.
- Preserve all existing Yes/No behavior.
- Keep the change minimal.

If the N/A → No conversion is NOT present in this file, do NOT invent a fix or modify unrelated code. Report that the conversion occurs outside this file.

After editing, show the exact diff.

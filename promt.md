Bug 206 fix — Option A. Make the EXCEPTION MITIGATION dropdown and Policy Exceptions comments immediately usable in edit mode WITHOUT the user first clicking "Add Policy Exception". Frontend only. Show diffs before applying. Do NOT change the backend contract.

FILE: frontend/src/app/review/[ecif]/review-info/components/sections/PolicyExceptionsSection.tsx

Approach (agent's recommended minimal fix #3): when the section is in edit mode and there are zero policy-exception rows, auto-seed ONE blank row and select it, so `active` is never null in edit mode. This unblocks both the Mitigation dropdown (line ~631) and the comments RichTextEditor (line ~658) without changing their onChange guards.

Requirements:
1. In the auto-select / initialization effect (lines ~194-207), when isEditing is true and rows.length === 0, create one blank row (via the same addRow used by handleAddPolicyException, ~155-177) and setSelectedId to it. Only seed once; do not re-seed on every render or fight the user if they delete all rows intentionally — guard it so it seeds only on entering edit mode with no rows.
2. Default the seeded row's exception type so it can be saved without error. Use the existing "E00 - No Policy Exceptions" type that's already special-cased (lines ~522-527) as the default typeCode, so mitigation/comments-only edits persist under a valid key.
3. IMPORTANT — do not create junk records: if the user makes NO edits (mitigation stays default "Not Mitigated", notes empty, type still E00 default and untouched), the seeded blank row must NOT be sent on Save. Ensure an untouched seeded row is treated as empty and skipped by the save path (page.tsx ~828-915), so we don't persist empty policy exceptions every time someone opens the section in edit mode.

Do NOT remove the isEditing gate on the fields. Do NOT change page.tsx save keying (still keyed by exceptionTypeCode). Do NOT touch the backend.
List every file + line changed. 
Commit: "Fix Bug 206: auto-seed a policy exception row in edit mode so Mitigation/comments are editable without adding a record first".


Implement ONLY UAT Item 52 based on the approved read-only diagnosis below.
Do not work on any other UAT item in this task.

==================================================
UAT ITEM 52 — APPROVED FOR CODE EDITS
==================================================
Screen/Tab: Review Form / CRM Findings
Section/Field: Finding Description

Business requirement:
The top “Finding Description” section in CRM Findings must update based on which CRM Finding row the user clicks / focuses / activates, matching legacy Access behavior.

Approved diagnosis / implementation direction:
- The top Finding Description is currently rendered in:
  frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx
- It currently displays s.findingDescription, which is a single form-level value from useCrmFindings().
- This is incorrect for UAT 52 because the description should reflect the currently active CRM Finding row, not a static form-level value.
- The safest minimal fix is a UI-only change in CrmFindingsAndRatingsSection.tsx.
- Do NOT change backend contracts, save payloads, hook response shape, or unrelated business logic.

==================================================
IMPLEMENTATION TO APPLY
==================================================

Make the change ONLY in:
- frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

Do NOT modify useCrmFindings.ts unless absolutely required for a compile fix. The intended fix is UI-only in the section component.

--------------------------------------------------
1) Add local active row state
--------------------------------------------------
Inside CrmFindingsAndRatingsSection.tsx, add local state to track the currently active CRM Finding row.

Use:
- activeRowId: string | null

Behavior:
- Default null initially.
- When findings rows load/change, initialize or revalidate activeRowId:
  - If current activeRowId still exists in s.findings, keep it.
  - Else prefer the first row that has a findingCode.
  - Else use the first row if any row exists.
  - Else set null.

This state is local UI state only. It must not be persisted and must not affect save payloads.

--------------------------------------------------
2) Set active row from user interaction
--------------------------------------------------
When the user interacts with a CRM Finding row, that row must become the active row for the top Finding Description panel.

Add active-row setting in BOTH ways:

A) Row click
- On the row container / <tr> for each CRM finding row, add an onClick that sets:
  setActiveRowId(row.id)

B) Focus inside a row
- For controls inside a row, add onFocus handlers that also set:
  setActiveRowId(row.id)

Apply onFocus to the row’s interactive controls, at least:
- CRM Component select
- Finding Code select
- Info input/button if focusable
- Severity select
- Findings Comments editor wrapper / focusable input area
- Follow-up checkbox if present

Goal:
- Mouse click and keyboard/tab navigation should both update the active row.

--------------------------------------------------
3) Compute the top Finding Description from the active row
--------------------------------------------------
Replace the current top description rendering logic that uses:
- s.findingDescription

New behavior:
- Find the active row from s.findings using activeRowId.
- If there is no active row, show blank/empty fallback.
- If active row has no component or no findingCode, show blank/empty fallback.
- Otherwise derive the description from the existing FINDING_LABELS / labelOpts already returned by the hook.

Current available data in this component:
- s.FINDING_LABELS is exposed as labelOpts (or equivalent local alias)
- labelOpts is keyed like:
  labelOpts[component][findingCode] => label string
- Label format is already built by the hook as:
  "CODE - DESCRIPTION" when description exists
  or just "CODE" when description is missing

Use that existing label map. Do NOT introduce a new API call.

Implementation rule for extracting the top description:
1. Get label string from:
   labelOpts?.[activeRow.component]?.[activeRow.findingCode]
2. If no label exists, render blank/empty fallback.
3. If label contains " - ", display only the substring AFTER the first " - " as the top Finding Description.
4. If label does not contain " - " (code only / no description), render blank/empty fallback.

Examples:
- "DI-101 - PD and/or LGD grade(s) needs updating"
  => top Finding Description should render:
     "PD and/or LGD grade(s) needs updating"

- "CRM-00"
  => no description part exists
  => render blank/empty fallback

--------------------------------------------------
4) Render behavior requirements
--------------------------------------------------
The top Finding Description panel must now behave as follows:

- If user clicks row A, panel shows row A’s finding description.
- If user tabs/focuses into row B, panel updates to row B’s finding description.
- If row has findingCode but no description exists in label map, panel should be blank.
- If no rows exist, panel should be blank.
- If active row is deleted and no longer exists, active row should be recalculated by the initialization/revalidation logic above.

Do not display synthetic placeholder text unless the current UI already requires it.
Prefer blank rendering for empty/fallback state.

--------------------------------------------------
5) Preserve existing behavior
--------------------------------------------------
Do NOT change:
- Existing row update behavior
- Existing save behavior / FormChangesContext behavior
- Existing findings library fetch logic
- Existing dropdown option rendering
- Existing item 53/54/55/56 behavior
- Existing hook contracts unless required for compile only

This is a UI read-model change only for the top Finding Description display.

==================================================
IMPORTANT IMPLEMENTATION CONSTRAINTS
==================================================
1. Work ONLY on UAT Item 52.
2. Do NOT implement or partially implement UAT 53, 54, 55, or 56.
3. Keep the change minimal and isolated.
4. Do not refactor unrelated code.
5. Do not add debug logs.
6. Do not alter API calls or save payloads.
7. Prefer small helper constants/functions inside the component if needed, but do not over-engineer.

==================================================
EXPECTED OUTPUT FROM YOU
==================================================
Make the code change and then report back in exactly this format:

### UAT Item 52 – Implemented

1. Files changed
- <file path>

2. What changed
- <concise summary>

3. Active row behavior implemented
- <details>

4. Finding Description resolution logic
- <details>

5. Edge cases handled
- <details>

6. Confirmation
- Confirm that ONLY UAT Item 52 was changed and no other UAT items were modified.

==================================================
FINAL REMINDER
==================================================
Implement ONLY the approved UAT 52 behavior in CrmFindingsAndRatingsSection.tsx.
Do not touch other UAT items.

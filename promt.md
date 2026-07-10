Work on ONLY UAT Item 52. Do not touch any other UAT item in this task.

==================================================
UAT ITEM 52
==================================================
Screen/Tab: Review Form / CRM Findings
Section/Field: Finding Description

Business requirement:
The top “Finding Description” section in CRM Findings must update based on which CRM Finding row the user clicks / highlights / focuses, matching the legacy Access behavior.

Expected behavior:
1. When the user clicks, focuses, or otherwise activates a CRM Findings row, the top Finding Description area should display the description for that row’s currently selected Finding Code.
2. The description shown must correspond to the active row only, not a static form-level value.
3. If the active row has no selected finding code or no description is available, show an appropriate empty/fallback state.
4. The solution must be minimal-risk and should reuse existing CRM Findings data / findings library logic if possible.

==================================================
STRICT WORKING RULES
==================================================
1. Work ONLY on UAT Item 52.
2. Do NOT implement or modify UAT Items 53, 54, 55, or 56 in this task.
3. First do a READ-ONLY diagnosis and STOP before making any code changes.
4. After diagnosis, wait for approval before editing code.
5. Prefer the smallest safe change.
6. Do not change save payload semantics unless absolutely required.
7. Do not add temporary debug logging unless explicitly approved.
8. Preserve existing business logic outside the exact scope of Item 52.

==================================================
KNOWN CONTEXT FROM PRIOR INVESTIGATION
==================================================
Use these findings unless current code proves otherwise.

Likely frontend files involved:
- frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx
- frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useCrmFindings.ts

Relevant CRM Findings context:
- Finding Code dropdown options come from FINDING_CODE_OPTIONS / codeMap
- Findings labels / descriptions may be derived from findings library data in useCrmFindings
- Findings library records include fields such as:
  - component
  - findingCode
  - description
  - guidance

Legacy Access behavior from UAT screenshots:
- The top “Finding Description” section is NOT static.
- It changes depending on which CRM Findings row is selected / highlighted.
- Example behavior:
  - Clicking/highlighting a row changes the top description panel to that row’s finding description.

Design expectation for the web app:
- The top Finding Description must reflect the currently active CRM Findings row.
- If the active row has a selected finding code, the description should come from that selected finding.
- If no row is active or no finding is selected, show a safe empty/fallback state.

==================================================
YOUR TASK — READ-ONLY DIAGNOSIS ONLY
==================================================
Do NOT edit code yet.

Investigate and answer ALL of the following:

1) Where exactly is the top “Finding Description” section rendered?
- file path
- JSX block / component location
- what state / prop / value currently populates it

2) What currently drives the top Finding Description value?
Determine whether it is currently:
- a single form-level field
- derived from a selected row
- derived from current finding code
- derived from review response data
- derived from findings library lookup
- placeholder / stale / unrelated value

3) How are CRM Findings rows represented today?
Identify:
- where row state lives
- the row shape / important fields
- whether each row already contains component, findingCode, findingDescription, etc.
- whether there is already any selected/active/highlighted row state

4) What user interactions currently happen on CRM Findings rows?
Identify the relevant handlers/events for:
- row click
- focus inside a row
- CRM Component change
- Finding Code change
- any row update function
- any state that already tracks “current row” implicitly

5) What is the safest minimal way to identify the active row for Item 52?
Evaluate the best low-risk implementation approach, for example:
- store activeRowId when the user clicks a row
- store activeRowId when any input/select in a row receives focus
- reuse an existing selected-row mechanism if one already exists
- derive from the most recently interacted row if that is already tracked

You must explicitly recommend ONE approach and explain why it is the smallest safe fix.

6) Once the active row is known, what is the correct source for the description?
Determine the best source in current codebase:
- directly from row state, if already available
- from existing findings label/library data in useCrmFindings
- from findings library lookup keyed by component + findingCode
- from review response if already present

Be explicit about:
- how description will be resolved for the active row
- what fallback should happen if component/findingCode/description is missing

7) Propose the smallest safe implementation for UAT Item 52 only.
Your proposal must clearly state:
- exact file(s) to change
- whether any new state is needed (e.g. activeRowId)
- where that state should live
- which event should set the active row
- how the top Finding Description text will be computed
- what fallback/empty behavior will be shown
- why this does NOT require changing unrelated UAT items

==================================================
RESPONSE FORMAT — MANDATORY
==================================================
After diagnosis, STOP and respond in exactly this format:

### UAT Item 52 – Read-only Diagnosis

1. Current implementation
- <detailed explanation>

2. Root cause / gap
- <detailed explanation>

3. Exact files involved
- <file 1>
- <file 2>
- ...

4. Smallest safe fix
- <proposed implementation plan>

5. Risk assessment
- <risk / no-risk notes>

6. Recommendation
- <final recommendation>

7. STOPPED – awaiting approval before code edits

==================================================
IMPORTANT STOP CONDITION
==================================================
Do not make any code changes in this run.
Do not mix in Item 53/54/55/56.
Stop immediately after the read-only diagnosis and wait for approval.

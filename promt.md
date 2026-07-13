UAT #29 — Customer Info: Relationship Manager & Portfolio Manager dropdowns from Data Mart Trial.

Client has confirmed the requirements:
1. "Let's use the Data Mart Trial table and we can revisit this later if Active Directory connectivity can be established."
2. "Store both the RM Name and Number in their respective fields, as well as the PM Name and Number. Only the name will appear in the Relationship Manager and Portfolio Manager name fields on-screen."
3. "Let's show both (e.g., 43724 – Geoffrey Houlditch)" in the dropdown.

DB (LIVE DB — ignore columns.csv, it is stale):
- Source table: dbo.[01_DATA_01_Data Mart Trial]
  - RM: [OfficerName] + [OfficerNumber]
  - PM: [PMName] + [PM Number]
- Target fields on the review: Relationship_mgr_name + Relationship_mgr_number, Portfolio_mgr_name + Portfolio_mgr_number

Behaviour:
- The Relationship Manager and Portfolio Manager fields on Customer Info become dropdowns sourced from Data Mart Trial (distinct name+number pairs).
- Dropdown option LABEL: "NUMBER – NAME" (e.g. "43724 – Geoffrey Houlditch").
- On save, BOTH the name and the number must persist to their respective columns.
- The saved value must display correctly on reload (if a saved value is no longer in the list, keep it visible — use the existing ensureIncludesSelected-style fallback rather than showing blank).

YOUR TASK — report FIRST (read-only, no edits), then propose the plan:

1. Frontend: show how the Relationship Manager and Portfolio Manager fields currently render in CustomerInfoSection.tsx, and what they are currently bound to (the CAS Users library). Show the exact JSX and the save staging.

2. Backend: 
   a. Confirm the exact column names and data types on dbo.[01_DATA_01_Data Mart Trial] for OfficerName/OfficerNumber/PMName/[PM Number] — give me the SQL to verify.
   b. Confirm the target columns Relationship_mgr_name / Relationship_mgr_number / Portfolio_mgr_name / Portfolio_mgr_number — which table are they on, and are they already in the review GET payload and the SAVE path? Show the current read and write for them.
   c. We need a new lookup endpoint returning distinct RM and PM name+number pairs. REUSE the existing LookupsController / ReportingService pipeline (the same pattern used for the distribution-party-names and relationship segments/units/markets endpoints) rather than building a new controller. Show me how those existing endpoints are wired so we follow the same pattern.

3. Propose the implementation plan file by file, in the ORDER to apply it (backend first, then frontend), so I can test incrementally.

Report findings and plan. STOP and wait for approval.

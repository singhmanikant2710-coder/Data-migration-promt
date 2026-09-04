Bug 223 part 1 fix — CAS Findings edit fires INSERT causing PK violation. The table PK is Finding_code ALONE (not composite). Fix so edit is always a real UPDATE keyed on Finding_code. Show all diffs before applying. Do NOT touch the Add/Create-new-finding flow or DeleteLibraryItem used by the row Delete button.

=== BACKEND ===
FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlFindingsRepository.cs

1. UpdateLibraryItemAsync (lines 125-153): The UPDATE currently matches on Finding_CRM_component + Finding_code (over-restrictive; component is editable and NOT part of the PK). Change it to:
   - Match ONLY on Finding_code (the real PK): WHERE LTRIM(RTRIM([Finding_code])) = LTRIM(RTRIM(@code))
   - Also allow updating the component in the SET clause, since edit may change it: add [Finding_CRM_component] = @comp to SET.
   So the UPDATE sets Finding_CRM_component, Finding_description, Finding_category, Finding_guidance, Finding_group, Active — matched on Finding_code only. Keep the existing "0 rows → KeyNotFoundException" behavior if present.

Do NOT change CreateLibraryItemAsync's INSERT columns, but note it stays only for the true Add flow.

=== BACKEND CONTROLLER ===
FILE: backend/src/Casrr.Api/Controllers/FindingsController.cs
The PUT route is library/{component}/{code}. Since we now update by code and can change component, ensure the update DTO/handler passes the NEW component to the repo. If UpdateCasFindingLibraryItemDto lacks Component, add an optional Component field and pass it through to UpdateLibraryItemAsync. The {component} route segment identifies the existing row's current component only; the new component comes from the body. Do not break the existing route.

=== FRONTEND ===
FILE: frontend/src/app/maintenance/cas-findings/page.tsx

2. handleSave (lines 359-447): REMOVE the compChanged create-then-delete branch entirely (lines ~365-378). Edit must ALWAYS call updateLibraryItem (the else branch), passing the (possibly changed) component in the payload so the backend can update it. So the save always does:
   await updateLibraryItem(row.original.findingCode, { component: row.component, description, category, guidance, findingGroup, active });
   (Adjust updateLibraryItem signature/casFindings.ts service + route call so the row is identified by findingCode, and component travels in the body. If the route still needs the old component segment, pass row.original.component as the route segment and row.component in the body.)

3. Fix the duplicate-option defect: componentOptions build (lines ~231-242) trims values, but line ~1194 injects the untrimmed row.original.component via new Set([...componentOptions, r.original.component]), creating a visually identical duplicate option. Change it to inject the TRIMMED original: (r.original.component ?? '').trim(), so there is exactly one option per component and compChanged can't flip on an identical-looking twin. (This defensive fix stays valid even though the create-then-delete branch is gone.)

Do NOT change: the Add New Finding flow, the row Delete button/deleteLibraryItem, or any other maintenance page.
List every file + line changed. 
Commit: "Fix Bug 223: CAS Findings edit updates by Finding_code (real PK) instead of create+delete; dedupe component options".

Bug 223 fix — Geoff confirmed Option 1. Two changes in the CAS Findings library maintenance. Show all diffs before applying. Do NOT touch Add New Finding or Delete flows.

PART 1 — Make Component read-only in edit; edit is always a real UPDATE (fixes the PK error).
FILE: frontend/src/app/maintenance/cas-findings/page.tsx
1. handleSave (~lines 359-447): REMOVE the compChanged create-then-delete branch entirely (~365-378). Edit must ALWAYS call updateLibraryItem with the ORIGINAL unchanged component + code:
   await updateLibraryItem(row.original.component, row.original.findingCode, { description, category, guidance, findingGroup, active });
2. In EDIT mode, render the Component dropdown AND the Finding Code field as DISABLED / read-only. Editable in edit mode: Category, Description, Guidance, Finding Group, Active only.
3. Remove the now-dead compChanged logic.
FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlFindingsRepository.cs
4. UpdateLibraryItemAsync: keep as a real UPDATE matching on Finding_CRM_component + Finding_code, setting only editable fields (description, category, guidance, group, active). Safe now that component is immutable in edit. Keep 0-rows → KeyNotFoundException.

PART 2 — Column reorder (display only).
FILE: frontend/src/app/maintenance/cas-findings/page.tsx
5. Move the "Finding Group" column so the table reads left-to-right: Component | Finding Code | Finding Group | Category | Description | Guidance | Active. Move BOTH the header and the corresponding cell in each row together, keeping their existing widths/styles. This is display-order only — do NOT change data, keys, or any field logic.

Do NOT change CreateAsync, the Add New Finding UI, DeleteLibraryItemAsync, or the row Delete button.
List every file + line changed. Commit: "Fix Bug 223: CAS Findings Component/Code read-only in edit (always UPDATE); reorder Finding Group column before Category".

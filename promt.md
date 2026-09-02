READ-ONLY DIAGNOSTIC — Bug 196 (CASRR UAT)

STRICT RULES:
- READ-ONLY. Do NOT edit, create, or delete any file.
- Do NOT run the app or apply any change.
- Auto-approve is OFF. Only read/open files and report back.
- Goal is diagnosis only. No fixes yet.

BUG CONTEXT:
Screen/Tab: Libraries > Selections
Section: CAS Findings library > CRM Component category
Symptom: User cannot edit the "Reporting name" selection.
On save, a PRIMARY KEY error is returned.

TRACE THIS FLOW END-TO-END (read only, report findings):

1. FRONTEND
   - Find the Selections maintenance component for the CAS Findings / CRM Component library.
   - Locate the "Reporting name" edit + save handler.
   - Identify the exact apiClient call: HTTP method (POST vs PUT), URL, and payload keys sent on save.
   - Confirm whether edit uses an UPDATE path or accidentally hits a CREATE/INSERT path.

2. BACKEND — CONTROLLER
   - Find the controller endpoint that receives this save.
   - Note route, HTTP verb, and the request DTO it binds to.

3. BACKEND — APPLICATION LAYER
   - Find the Command/Handler (or service method) invoked.
   - Determine if it performs INSERT (Add) or UPDATE.
   - Check how it resolves the existing record: by which key(s)?

4. BACKEND — INFRASTRUCTURE / EF
   - Find the Entity + EF configuration for the CRM Component / CAS Findings Selection table.
   - Report the PRIMARY KEY definition (single or composite? e.g. LibraryId + ComponentId + ReportingName?).
   - Check if the PK includes a column that is being CHANGED during edit (e.g. ReportingName itself is part of the key) — this would cause a PK violation on edit.
   - Check whether SaveChanges tracks the entity as Added vs Modified.

REPORT BACK (do not fix):
- Exact file paths + line numbers for each layer.
- The exact PRIMARY KEY definition of the affected table.
- Root cause hypothesis: is it (a) INSERT firing instead of UPDATE, (b) a mutable column being part of the PK, or (c) a detached/re-added entity being tracked as Added?
- Confirm which one, with evidence from the code.

Do NOT propose or write any fix yet. Just report the diagnosis.

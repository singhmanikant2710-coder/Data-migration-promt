Bug 196 fix. Apply exactly these changes. Do NOT touch UpdateAsync, DeleteAsync, or the Edit save path. Do NOT change existing method signatures. Show me each diff before applying.

=== FILE 1 ===
backend/src/Casrr.Infrastructure/Repositories/SelectionRepository.cs
Method: CreateAsync ONLY.

The client sends Selection_id, which collides with the existing composite PK (Section, Selection_id) and causes a Primary Key violation. Fix: generate Selection_id server-side as the next per-section value inside the SAME connection as the INSERT, so MAX+INSERT is atomic. Keep the existing existsSql duplicate guard as-is (safety net).

Replace the INSERT portion of CreateAsync so that:
1. Open ONE connection.
2. First run this scalar to get the next id for that section:
   SELECT ISNULL(MAX([Selection_id]), 0) + 1
   FROM dbo.[03_LIBRARY_09_Selections] WITH (UPDLOCK, HOLDLOCK)
   WHERE LTRIM(RTRIM([Section])) = LTRIM(RTRIM(@section));
   Read it into an int newId.
3. Then run the existing INSERT, but bind @id = newId (NOT item.SelectionId):
   INSERT INTO dbo.[03_LIBRARY_09_Selections] ([Selection_id], [Tab], [Section], [Selection])
   VALUES (@id, @tab, @section, @selection);
4. Wrap steps 2-3 in a transaction so the MAX read and INSERT are atomic.
Keep @tab, @section, @selection bindings exactly as they already are. Keep the "rows <= 0" check.

=== FILE 2 ===
frontend/src/app/maintenance/selections/page.tsx
Function: handleCreateInline ONLY (the inline Add save path).

Selection_id is now server-generated, so stop requiring/sending it from the user:
1. Remove this validation block:
   const idNum = Number(idVal);
   if (!Number.isFinite(idNum) || idNum <= 0) {
     setCreateError("Selection Id must be a positive integer.");
     return;
   }
2. In the createSelection call, remove selectionId (or send selectionId: 0):
   const created = await createSelection({ tab, section, selection: selText });
3. In the inline Add UI, hide/remove the "Selection Id" input field and its label (the addId input). Do NOT remove the Section dropdown or Selection text input.
Do NOT change the Edit row rendering or the Edit save path.

=== IF createSelection SIGNATURE REQUIRES selectionId ===
If the createSelection service function or its request DTO makes selectionId required, make it optional (default 0) rather than changing any caller. Show me that change too.

After applying, list every file you changed. Commit: "Fix Bug 196: server-side per-section Selection_id generation; remove client-supplied id from Add flow".

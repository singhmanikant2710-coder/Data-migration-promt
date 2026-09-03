Bug 196 fix. TWO small changes only. Do NOT touch UpdateAsync, DeleteAsync, or the Edit save path.

FILE 1: backend/src/Casrr.Infrastructure/Repositories/SelectionRepository.cs — CreateAsync only.
The client-supplied Selection_id causes duplicate composite-key (Section, Selection_id) collisions. Change CreateAsync so the Selection_id is ALWAYS generated server-side as the next per-section value, ignoring item.SelectionId:

Before the INSERT, in the same connection, run:
  SELECT ISNULL(MAX([Selection_id]), 0) + 1
  FROM dbo.[03_LIBRARY_09_Selections] WITH (UPDLOCK, HOLDLOCK)
  WHERE LTRIM(RTRIM([Section])) = LTRIM(RTRIM(@section));
Use the returned value as @id in the INSERT (do a single connection/transaction so the MAX+INSERT is atomic). Keep the existing existsSql duplicate guard as a safety net. Keep method signature and INSERT columns unchanged.

FILE 2: frontend/src/app/maintenance/selections/page.tsx — handleCreateInline only.
Remove the requirement for the user to type a Selection Id. Stop sending selectionId from addId (it is now server-generated). In the createSelection call, drop selectionId (or send 0). Remove the "Selection Id must be a positive integer" validation gate for the Add flow, and hide/remove the Selection Id input in the inline Add UI. Do NOT change the Edit UI.

Show me both diffs. Commit: "Fix Bug 196: server-side per-section Selection_id generation for Add; remove client-supplied id".

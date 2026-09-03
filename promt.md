Option C — hold. Do NOT apply init→set on the Domain entity, and do NOT manually append the created row to React state. That append is exactly why selectionId 0 appears.

Instead:
- After a successful createSelection (201), call getSelections(selectedTab) to REFETCH the grid, and set that as the new rows state. Remove the code that appends the `created` object into state.
- This removes the need for the Domain init→set change entirely, because the refetched rows carry the real server-generated Selection_id.

So the final change set should be:
1. SelectionRepository.cs CreateAsync — server-side MAX+1 id generation.
2. SelectionsController — remove ONLY the SelectionId <= 0 400 guard (Blocker A / diff 3). This is required and fine.
3. page.tsx handleCreateInline — remove client Selection Id input + validation; on success, REFETCH via getSelections(selectedTab) instead of appending.

Do NOT touch the Domain entity (no init→set). Do NOT touch UpdateAsync, DeleteAsync, or Edit save path.
Show me all diffs. Confirm the Domain entity file is NOT in the changed list.

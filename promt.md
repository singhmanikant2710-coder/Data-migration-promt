You have now read FindingsController.cs,
the repository pattern, and cas-findings
page.tsx.
 
Now create the Selections feature following
EXACTLY the same patterns you just read.
 
DATABASE TABLE: [dbo].[03_LIBRARY_09_Selections]
Columns:
- Selection_id (int, PK, default 0)
- Tab (nvarchar 255, nullable)
- Section (nvarchar 255, nullable)
- Selection (nvarchar 255, nullable)
 
CREATE THESE FILES ONE BY ONE and confirm
each file path after creation:
 
FILE 1:
backend/src/Casrr.Domain/Entities/Selection.cs
(same pattern as existing domain entities)
 
After creating FILE 1, stop and confirm
the full file path. Wait for my approval
before FILE 2.
_________________________________________________________________________
FILE 1 confirmed. Now create FILE 2:
 
Create the repository interface following
exact same pattern as IFindingsRepository.
 
FILE 2:
backend/src/Casrr.Application/Interfaces/
ISelectionRepository.cs
 
Methods needed:
- GetAllAsync(string? tab = null)
- GetByIdAsync(int selectionId)
- GetDistinctTabsAsync()
- GetSectionsByTabAsync(string tab)
- CreateAsync(SelectionLibraryItem item)
- UpdateAsync(SelectionLibraryItem item)
- DeleteAsync(int selectionId)
 
After creating FILE 2, stop and confirm
the full file path. Wait for approval
before FILE 3.


------------------------------------------------------------------------------

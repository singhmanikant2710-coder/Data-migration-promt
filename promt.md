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
FILE 2 confirmed. Now create FILE 3:

Create the repository implementation following 
exact same pattern as FindingsRepository.

FILE 3:
backend/src/Casrr.Infrastructure/Repositories/
SelectionRepository.cs

Requirements:
- Implement ISelectionRepository
- Use same Dapper/EF pattern as 
  FindingsRepository
- Database table: [dbo].[03_LIBRARY_09_Selections]
- Column mappings:
  Selection_id → SelectionId
  Tab → Tab
  Section → Section
  Selection → Selection
- GetAllAsync: optional tab filter
  SELECT * FROM [03_LIBRARY_09_Selections]
  WHERE (@tab IS NULL OR Tab = @tab)
- GetDistinctTabsAsync:
  SELECT DISTINCT Tab FROM 
  [03_LIBRARY_09_Selections]
  WHERE Tab IS NOT NULL ORDER BY Tab
- GetSectionsByTabAsync:
  SELECT DISTINCT Section FROM 
  [03_LIBRARY_09_Selections]
  WHERE Tab = @tab AND Section IS NOT NULL
  ORDER BY Section
- Inject same DbConnection/DbContext as 
  existing repositories

After creating FILE 3, stop and confirm 
full file path. Wait for approval 
before FILE 4.

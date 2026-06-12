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


FILE 3 confirmed. Now create FILE 4:

Create the SelectionsController following 
exact same pattern as FindingsController.cs
that you already read.

FILE 4:
backend/src/Casrr.Api/Controllers/
SelectionsController.cs

Requirements:
- Route: "api/v1/selections"
- Authorize policy: "RequireActiveUser"
- Inherit: BaseTemplateController
- Inject: ISelectionRepository, ILogger, 
  TelemetryClient, IGraphUserInfoProvider
- Same error codes pattern as 
  FindingsController

ENDPOINTS:
1. GET api/v1/selections/library
   - Optional ?tab= filter
   - Returns list of SelectionLibraryItem

2. GET api/v1/selections/library/{id}
   - Returns single item, 404 if missing

3. POST api/v1/selections/library
   - Validates Tab + SelectionId unique
   - 201 on success, 409 on duplicate

4. PUT api/v1/selections/library/{id}
   - Updates item, 404 if missing

5. DELETE api/v1/selections/library/{id}
   - Deletes item, 404 if missing

6. GET api/v1/selections/tabs
   - Returns distinct Tab values

7. GET api/v1/selections/sections?tab={tab}
   - Returns distinct Section values 
     filtered by Tab

After creating FILE 4, stop and confirm 
full file path. Wait for approval 
before FILE 5.



FILE 4 confirmed. Now create FILE 5:

Register SelectionRepository in dependency 
injection following exact same pattern as 
FindingsRepository registration.

FILE 5:
Check where IFindingsRepository is registered
in the project (likely in):
backend/src/Casrr.Infrastructure/
DependencyInjection.cs
OR
backend/src/Casrr.Api/Extensions/
StartupExtensions.cs

DO NOT modify existing files.
Just tell me the exact file path where 
IFindingsRepository is registered so I 
can confirm before you add 
ISelectionRepository registration.

Confirm the file path only. 
Do not edit anything yet.

FILE 5 confirmed location.

Now add ISelectionRepository registration 
in StartupExtensions.cs following exact 
same pattern as IFindingsRepository 
registration in that file.

This requires modifying existing file:
backend/src/Casrr.Api/Extensions/
StartupExtensions.cs

Add ONLY these two lines in the correct 
place (where other repositories are 
registered):

services.AddScoped<ISelectionRepository, 
SqlSelectionRepository>();

Do not change anything else in the file.
After adding, confirm the line number 
where it was added.




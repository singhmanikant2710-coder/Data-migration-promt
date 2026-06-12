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


FILE 5 confirmed. Now create FILE 6:

Create the frontend API service file 
following exact same pattern as:
frontend/src/services/findingsService.ts
(or whatever the cas-findings service 
file is named)

FILE 6:
frontend/src/services/selectionsService.ts

Functions needed:

1. getSelections(tab?: string)
   GET /api/v1/selections/library
   optional ?tab= filter

2. getSelectionById(id: number)
   GET /api/v1/selections/library/{id}

3. createSelection(data: CreateSelectionDto)
   POST /api/v1/selections/library

4. updateSelection(id: number, 
   data: UpdateSelectionDto)
   PUT /api/v1/selections/library/{id}

5. deleteSelection(id: number)
   DELETE /api/v1/selections/library/{id}

6. getDistinctTabs()
   GET /api/v1/selections/tabs

7. getSectionsByTab(tab: string)
   GET /api/v1/selections/sections?tab={tab}

TypeScript interfaces needed:
- SelectionLibraryItem {
    selectionId: number
    tab: string | null
    section: string | null
    selection: string | null
  }
- CreateSelectionDto {
    tab: string
    section: string
    selectionId: number
    selection: string
  }
- UpdateSelectionDto {
    tab?: string
    section?: string
    selection?: string
  }

Use exact same axios/fetch pattern as 
existing service files.

After creating FILE 6, stop and confirm 
full file path. Wait for approval 
before FILE 7.


FILE 6 confirmed. Now create FILE 7:

Create the frontend page following exact 
same pattern as:
frontend/src/app/maintenance/cas-findings/
page.tsx

FILE 7:
frontend/src/app/maintenance/selections/
page.tsx

Page title: "Selections — Library Maintenance"

FILTER SECTION:
- "Filter by Tab" dropdown
- Populated from getDistinctTabs()
- Default: "All Tabs"
- On change: refetch list with tab filter

ADD NEW SELECTION FORM:
- Tab: dropdown (from getDistinctTabs())
- Section: dropdown 
  (cascades based on selected Tab,
  populated from getSectionsByTab(tab))
- Selection_id: number input
- Selection: text input
- Create button
- Validation: all fields required
- Show inline error on duplicate

SELECTIONS TABLE:
Columns:
Tab | Section | Selection_id | 
Selection | Actions

Actions per row:
- Edit: makes row inline editable
- Save: calls updateSelection()
- Delete: calls deleteSelection() 
  with confirmation dialog

Import service from:
frontend/src/services/api/selections.ts

Use exact same:
- Loading state pattern
- Error handling pattern
- TypeScript interfaces
- Component structure
as cas-findings page.tsx

After creating FILE 7, stop and confirm 
full file path. Wait for approval 
before FILE 8.


FILE 8 confirmed. Now for the final step:

Find the sidebar navigation file where 
maintenance menu items are listed.
It likely contains links to:
- cas-findings
- cas-users
- covenants
- policy-exceptions
- sample-criteria

First just tell me the exact file path 
of the sidebar/navigation file.
Do not edit anything yet.


All files have been created. Now do the 
final steps to make selections tab live:

STEP 1: Restart backend
Stop the current running backend and 
restart it with:
dotnet run --project backend/src/Casrr.Api/
Casrr.Api.csproj --launch-profile "Casrr.Api"

Wait for "Now listening on: 
http://localhost:5200" message.

STEP 2: Verify backend API works
Test this endpoint returns 200:
GET http://localhost:5200/api/v1/selections/library

STEP 3: Verify frontend page works
Check http://localhost:3100/maintenance/selections
returns 200 (not 404)

STEP 4: Confirm all working
Tell me:
✅ Backend restarted successfully
✅ GET /api/v1/selections/library = 200
✅ /maintenance/selections = 200

Do not stop until all 3 confirmations 
are green.

Do NOT disable authentication or restart 
backend with auth disabled.

The API returning 401 is correct and expected 
behavior - it means the endpoint exists and 
is working properly with auth.

Just open the browser and navigate to:
http://localhost:3100/maintenance/selections

The browser session already has auth token.
Tell me if the page loads or shows an error.

error Fix
Fix duplicate key error in:
frontend/src/app/maintenance/selections/page.tsx
line 486

Current code:
key={`${r.original.tab ?? ""}|${r.original.selectionId}`}

Replace with:
key={`${index}`}

Also add index parameter to the .map():
.map((r, index) => (

This will make every row key unique.
Only change line 486. Nothing else.



---- Making Best UI Through Below Promt--------
Redesign the Selections — Library Maintenance 
page at:
frontend/src/app/maintenance/selections/page.tsx

Make it look more professional and impressive 
than the current CAS Users/CAS Findings pages.
A senior developer should be proud of this UI.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DESIGN INSPIRATION:
Modern enterprise tools like 
Bloomberg Terminal, Salesforce Lightning, 
or Linear.app — clean, data-dense, 
professional.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. PAGE HEADER SECTION:
   - Left: Page title "Selections" in bold
     Subtitle: "Library Maintenance" 
     in smaller muted text below
   - Right: 
     • Search/filter input with search icon
     • "Filter by Tab" dropdown
     • "+ Add Selection" button 
       (filled navy blue, white text,
       rounded, with + icon)
   - Thin divider line below header

2. STATS BAR (below header):
   - Small pill badges showing:
     "Total: XX records"
     "Tabs: XX"  
     "Sections: XX"
   - Light gray background bar
   - Auto-calculated from data

3. TABLE DESIGN:
   - Sticky column headers
   - Header: dark navy (#1F3864) background,
     white text, uppercase small font
   - Columns: 
     Tab | Section | Selection Id | 
     Selection | Actions
   - Tab column: colored badge/pill 
     (different color per tab name)
   - Alternating row colors 
     (white / very light blue #F8FAFF)
   - Smooth hover effect on rows
     (light blue highlight)
   - Subtle bottom border on each row
   - Actions column: 
     Edit = outline blue button
     Save = filled green button  
     Delete = filled red button
     All small, compact, rounded

4. INLINE ADD ROW:
   - Click "+ Add Selection" → 
     highlighted yellow/amber row 
     appears at TOP of table
   - Row has: Tab dropdown, Section 
     dropdown (cascades), Selection Id 
     number input, Selection text input
   - "✓ Save" green button
   - "✗ Cancel" gray button
   - Row has distinct background to 
     stand out from data rows

5. INLINE EDIT ROW:
   - Click Edit → that specific row 
     turns light blue/highlighted
   - Fields become editable inputs
   - Save = green, Cancel = gray
   - Smooth transition animation

6. PAGINATION BAR:
   - Left: "Showing 1-10 of 55 results"
     in muted gray text
   - Center: Page number buttons
     (1, 2, 3... with current highlighted 
     in navy)
   - Right: 
     "Rows per page:" dropdown
     (10, 25, 50, 100)
   - Clean minimal style

7. EMPTY STATE:
   - If no data or filter returns nothing:
     Show centered icon + 
     "No selections found" message +
     "Clear filters" link

8. LOADING STATE:
   - Skeleton loader rows while fetching
     (gray animated placeholder rows)
   - Not just a spinner

9. DELETE CONFIRMATION:
   - Small modal popup (not browser alert)
   - "Delete Selection?" title
   - Shows the Selection text being deleted
   - "Cancel" outline button
   - "Delete" red filled button

10. TOAST NOTIFICATIONS:
    - Success: green toast bottom-right
      "Selection created successfully"
      "Selection updated successfully"  
      "Selection deleted successfully"
    - Error: red toast
      "Failed to save. Please try again."
    - Auto-dismiss after 3 seconds

11. RESPONSIVE:
    - Table scrolls horizontally on 
      small screens
    - Actions column stays sticky right

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
TECHNICAL:
- Use Tailwind CSS classes only
- Keep all existing API service calls
- Keep TypeScript interfaces same
- Add smooth CSS transitions
- No external UI libraries needed
- Match existing Next.js patterns

Only modify:
frontend/src/app/maintenance/selections/
page.tsx

Confirm file path after completion.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Fix the Selections Library table in:
frontend/src/app/maintenance/selections/page.tsx

CURRENT ISSUE:
All table rows show editable text inputs 
by default. This is wrong.

CORRECT BEHAVIOR (match CAS Users pattern):
- All rows should be READ-ONLY by default
- Tab column: plain text (green pill badge)
- Section column: plain text (not input)
- Selection Id: plain text (not input)
- Selection column: plain text (not textarea)
- Actions: only "Edit" and "Delete" buttons 
  visible by default
  (Save button hidden when not editing)

- When "Edit" clicked on a row:
  • ONLY that row becomes editable
  • Section becomes dropdown
  • Selection Id becomes number input
  • Selection becomes textarea
  • Save (green) and Cancel (gray) appear
  • Edit button hides
  • Delete button hides while editing

- All other rows remain read-only

This is the standard inline-edit pattern.
Only modify page.tsx. Confirm after fix.

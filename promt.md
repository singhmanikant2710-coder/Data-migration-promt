Using the existing CAS Findings maintenance tab
(/maintenance/cas-findings) as the code pattern
and architecture reference, implement full CRUD
for the Selections maintenance tab.
 
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DATABASE TABLE: [dbo].[03_LIBRARY_09_Selections]
Columns:
- Selection_id (int, PK, default 0)
- Tab (nvarchar 255, nullable)
- Section (nvarchar 255, nullable)
- Selection (nvarchar 255, nullable)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 
BACKEND (.NET Clean Architecture):
Follow exact same pattern as CAS Findings
backend files.
 
- Entity: Selection.cs
- Repository interface: ISelectionRepository
- Repository implementation: SelectionRepository
  (use same ORM/Dapper pattern as existing)
- Queries: GetAllSelections, GetSelectionById,
  GetDistinctTabs, GetSectionsByTab
- Commands: CreateSelection, UpdateSelection,
  DeleteSelection
- Controller: SelectionsController
  GET /api/selections (all, with optional
    ?tab= filter)
  GET /api/selections/{id}
  GET /api/selections/tabs (distinct Tab values)
  GET /api/selections/sections?tab={tab}
  POST /api/selections
  PUT /api/selections/{id}
  DELETE /api/selections/{id}
- Add proper exception handling, validation,
  and error responses matching existing
  controllers exactly
 
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 
FRONTEND (Next.js):
Route: /maintenance/selections
Page title: "Selections — Library Maintenance"
 
Layout matches cas-findings page structure BUT
with these specific fields:
 
1. FILTER SECTION (top):
   - "Filter by Tab" dropdown
   - Populated from GET /api/selections/tabs
   - Default: "All Tabs"
 
2. ADD NEW SELECTION FORM:
   - Tab: dropdown (from distinct Tab values)
   - Section: dropdown (cascades — filters by
     selected Tab via GET /api/selections/
     sections?tab={tab})
   - Selection_id: number input
   - Selection: text input
   - Create button
   - Validation: Tab + Selection_id must be
     unique. Show inline error if duplicate.
 
3. SELECTIONS TABLE (below form):
   Columns:
   Tab | Section | Selection_id | Selection |
   Actions
 
   Actions per row:
   - Edit button: makes row fields editable
     inline (same as cas-findings pattern)
   - Save button: calls PUT endpoint
   - Delete button: calls DELETE endpoint with
     confirmation
 
4. Add "Selections" link in sidebar navigation
   under Maintenance section (same position
   pattern as other maintenance items)
 
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 
STRICT RULES:
- DO NOT modify any existing files
- Only create new files
- Follow exact naming conventions of existing
  maintenance tabs
- Follow exact folder structure of project
- Match existing API service call patterns
  (axios/fetch — whichever is used)
- Match existing TypeScript interfaces pattern
- Match existing error handling pattern in UI
- Match existing loading state pattern in UI

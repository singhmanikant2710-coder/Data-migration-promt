I need to redesign the Loan Codes maintenance page to match the premium 
UI pattern used in the Selections page.

Read these files first (do not modify yet, just read):
1. frontend/src/app/maintenance/selections/page.tsx
2. frontend/src/app/maintenance/loan-codes/page.tsx
3. frontend/src/app/maintenance/loan-codes/components/LoanCodeTable.tsx
4. frontend/src/app/maintenance/loan-codes/types.ts

After reading, redesign loan-codes/page.tsx to match selections/page.tsx 
pattern exactly:

1. HEADER SECTION:
   - Title "Loan Codes" with subtitle "Library Maintenance" 
     (same style as Selections)
   - Search input on top right: "Search loan codes, category, code..."
   - Filter dropdown: "All Categories" (keep existing filter logic)
   - "+ Add Loan Code" button (dark navy, same style as "+ Add Selection")

2. STATS BAR:
   - Show TOTAL records count
   - Show CATEGORIES count (unique category count)
   - Same pill-style stat badges as Selections page

3. TABLE:
   - Dark navy header row (#1F3864) with columns: 
     CATEGORY | CODE | DESCRIPTION | ACTIONS
   - Replace the separate "Add New Loan Code" form box with an 
     inline amber-highlighted "add row" at the top of the table 
     (same pattern as Selections "+ Add Selection" inline row)
   - Category field: dropdown with existing categories + 
     "+ Add New Category" option
   - Code field: text input (required, must be unique)
   - Description field: text input
   - Row-level Save/Cancel buttons (green Save, outline Cancel)

4. EXISTING ROWS:
   - Category: dropdown (already exists, keep it)
   - Code: read-only text by default, editable on Edit click
   - Description: read-only text by default, editable on Edit click
   - Actions column: Edit (outline) + Delete (red) buttons
   - On Edit click, row becomes editable, Edit/Delete buttons 
     change to Save/Cancel

5. ADD THESE MISSING FEATURES (present in Selections but not here):
   - Pagination (10, 25, 50, 100 per page)
   - Skeleton loader while data loads
   - Delete confirmation modal (not direct delete)
   - Toast notifications on Save/Delete/Add (3 sec auto-dismiss)
   - Empty state message when no records match search/filter
   - Compact row styling (text-sm, consistent padding)

IMPORTANT RULES:
- Do not change any backend/API logic, only frontend UI/UX
- Do not modify selections/page.tsx or any other working tab
- Reuse existing shared components if Selections imports any 
  (e.g. Toast, DeleteModal, Pagination, SkeletonLoader) instead 
  of creating duplicates
- Keep all existing API calls and service functions in 
  services/api/loan-codes.ts as is, just update how the UI 
  consumes them
- Match exact spacing, font sizes, and button styles from 
  Selections page

Show me the complete modified page.tsx file when done, and tell me 
if LoanCodeTable.tsx is still needed or can be merged into page.tsx.

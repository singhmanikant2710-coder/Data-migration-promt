I need to redesign the Help Tips maintenance page to match the premium 
UI pattern used in Selections and Loan Codes pages, while keeping its 
unique rich text editor feature intact.

Read these files first (do not modify yet, just read):
1. frontend/src/app/maintenance/selections/page.tsx
2. frontend/src/app/maintenance/loan-codes/page.tsx
3. frontend/src/app/maintenance/help-tips/page.tsx
4. frontend/src/app/maintenance/help-tips/components/HelpTipTable.tsx
5. frontend/src/app/maintenance/help-tips/types.ts

After reading, redesign help-tips/page.tsx to match the Selections/
Loan Codes pattern:

1. HEADER SECTION:
   - Title "Help Tips" with subtitle "Library Maintenance"
   - Search input: "Search help tips, form, topic..."
   - Keep existing "Filter by Form" and "Filter by Topic" dropdowns, 
     but restyle them to match the filter dropdown style used in 
     Selections/Loan Codes
   - "+ Add Help Tip" button (dark navy, same style as other tabs)

2. STATS BAR:
   - TOTAL records count
   - FORMS count (unique form count)
   - TOPICS count (unique topic count)
   - Same pill-style stat badges as other Maintenance tabs

3. TABLE:
   - Dark navy header row (#1F3864) with columns: 
     FORM | TOPIC | HELP TIP | ACTIONS
   - Help Tip column should show a truncated/preview version of the 
     HTML content (strip tags, show first ~100 characters with "...") 
     in the read-only row view, not the full rendered HTML
   - Row-level Edit/Delete actions (Edit outline button, Delete red button)

4. ADD NEW HELP TIP FLOW:
   - IMPORTANT: Keep the rich text editor (B, I, U, P, H1, H2, DIV, 
     List, Img URL, Upload, Table, Link, Clear toolbar) exactly as is, 
     do not remove or simplify it
   - Instead of a separate "Add New Help Tip" card above the table, 
     trigger the add form via the "+ Add Help Tip" button — it can 
     either expand as a panel above the table (since the rich editor 
     needs more vertical space than a single table row) or open in 
     a modal/drawer
   - Form and Topic fields stay as required text inputs
   - Keep the rich text editor for Help Tip Content exactly as 
     currently implemented

5. EDIT EXISTING ROW:
   - On Edit click, show the same rich text editor inline or in 
     expanded row view (not just plain text input) so HTML 
     formatting can be edited
   - Save/Cancel buttons (green Save, outline Cancel)

6. ADD THESE MISSING FEATURES (present in Selections/Loan Codes 
   but not here):
   - Pagination (10, 25, 50, 100 per page)
   - Skeleton loader while data loads
   - Delete confirmation modal (not direct delete)
   - Toast notifications on Save/Delete/Add (3 sec auto-dismiss)
   - Empty state message when no records match search/filter
   - Compact row styling (text-sm, consistent padding) for the 
     table itself, even though the editor panel will need more space

IMPORTANT RULES:
- Do not change any backend/API logic, only frontend UI/UX
- Do not modify selections/page.tsx or loan-codes/page.tsx or 
  any other working tab
- Do not remove or break the rich text editor functionality 
  (formatting buttons, image upload, table insert, link insert)
- Reuse existing shared components (Toast, DeleteModal, Pagination, 
  SkeletonLoader) instead of creating duplicates
- Keep all existing API calls and service functions as is, just 
  update how the UI consumes them
- Match exact spacing, font sizes, and button styles from Selections 
  and Loan Codes pages

Show me the complete modified page.tsx file when done, and confirm 
whether HelpTipTable.tsx is still used or merged into page.tsx.

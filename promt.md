There's a UI bug in the NAICS maintenance page. The Division dropdown 
(and likely Sector dropdown too) opens UPWARD and covers the entire 
screen, instead of opening DOWNWARD in a contained way like the 
Category dropdown does on the Loan Codes page.

Read these files first:
1. frontend/src/app/maintenance/naics/page.tsx
2. frontend/src/app/maintenance/loan-codes/page.tsx

Compare how the dropdown/select component is implemented in both files.

ISSUE:
- In loan-codes/page.tsx, the Category dropdown opens downward, 
  is properly contained, and doesn't cover other UI elements.
- In naics/page.tsx, the Division dropdown opens upward and 
  expands to cover the full screen height, which is broken behavior.

FIX NEEDED:
1. Check the dropdown/Select component used in naics/page.tsx 
   (likely a custom Select or similar imported component)
2. Compare its props/configuration against the same component 
   used in loan-codes/page.tsx for the Category field
3. Identify what's different — likely one of these:
   - Missing or incorrect `position` prop (e.g. should be "popper" 
     with side="bottom" instead of "top" or "auto")
   - Missing `position: relative` on the parent table row/cell 
     container, causing the dropdown to calculate position incorrectly
   - Missing max-height or incorrect z-index causing it to expand 
     full screen
   - If using a library like Radix/shadcn Select, check the 
     `SelectContent` component for `position`, `side`, and 
     `sideOffset` props
4. Fix the Division dropdown and Sector dropdown in naics/page.tsx 
   to match the exact same dropdown behavior, sizing, and downward 
   opening direction as the Category dropdown in loan-codes/page.tsx

IMPORTANT:
- Do not modify loan-codes/page.tsx (it's working correctly)
- Do not change any data/API logic, this is purely a UI positioning fix
- Keep the same dropdown options and "+ Add New Division" / 
  "+ Add New Sector" functionality intact
- Test that the dropdown stays within table row bounds and 
  doesn't overflow/cover the whole screen

Show me the specific lines that were changed in naics/page.tsx.

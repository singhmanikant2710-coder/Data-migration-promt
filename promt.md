Improve the Review History page to match the PPT design more 
precisely and make it visually premium. Do NOT change any data 
logic, API calls, or search functionality.

Read this file: frontend/src/app/review-history/page.tsx

Also read frontend/src/app/review-queue/page.tsx to see if it has 
Home/Refresh/Close icons in its header — if so, copy that exact 
pattern.

MAKE THESE CHANGES:

1. HEADER BAR (top navy bar with "REVIEW HISTORY" text):
   - Currently only has a "Home" icon/link on the right
   - ADD two more icon buttons next to Home, matching the PPT:
     - Refresh icon (circular arrow) — onClick should re-fetch 
       the current search results (call the same fetch function 
       used on Search button click, with current filter values)
     - Close icon (X, in a red/danger colored button) — onClick 
       should navigate back to the home page ("/")
   - Match icon button styling to whatever icon buttons already 
     exist elsewhere in the app (check review-queue/page.tsx or 
     Button component variants)

2. TABLE CONTAINER:
   - Add a max-height to the table body (e.g. max-h-[500px] or 
     similar) with overflow-y-auto so the table scrolls internally 
     when there are many rows, instead of pushing the whole page 
     down indefinitely
   - This matches the PPT which shows a visible scrollbar on the 
     right side of the table

3. PAGINATION:
   - Add pagination below the table (10, 25, 50, 100 per page 
     options), reusing the exact same Pagination component used 
     in selections/page.tsx or loan-codes/page.tsx
   - Do not build a new pagination component

4. PREMIUM ALIGNMENT/SPACING:
   - Review the column spacing/padding in the table — ensure 
     consistent padding (px-4 py-3 or similar) across all columns, 
     matching the polish level of review-queue/page.tsx or 
     selections/page.tsx
   - Ensure the search bar section (Sample dropdown + Borrower 
     input + Search button) has consistent spacing/alignment with 
     the rest of the Maintenance tabs' search bars
   - Add a stats bar below the search bar showing "TOTAL: X records" 
     (matching the pill-style badge used in selections/page.tsx)

5. BORROWER NAME LINK BEHAVIOR (matching PPT annotation #3):
   - In review-queue/page.tsx, check how the borrower name link 
     and linesheet icon work together (the handleOpen pattern)
   - For Review History, since this is read-only and finalized, 
     keep the borrower name as plain text (not clickable, since 
     there's no live review to navigate to) — but make the 
     linesheet icon button open the same ReviewPDFModal as before, 
     which is the only "link-like" action that makes sense for 
     finalized reviews
   - Confirm this approach is reasonable given the read-only nature 
     of finalized reviews, or flag if the PPT's annotation #3/#4 
     implies something else for finalized reviews specifically

IMPORTANT RULES:
- Do not modify review-queue/page.tsx, selections/page.tsx, or 
  loan-codes/page.tsx
- Do not change any API/data fetching logic, only UI/layout
- Reuse existing shared components (Pagination, icon buttons, 
  stats badges) — do not create duplicates

Show me the complete updated page.tsx file when done.

Create the Review History page UI. Do NOT modify any existing file.

Read these files first (do not modify):
1. frontend/src/app/review-queue/page.tsx
2. frontend/src/services/api/reviewHistory.ts (just created)
3. The shared DataTable component (search for "@/components/table/DataTable")

CREATE this new file ONLY:
frontend/src/app/review-history/page.tsx

UI SPECIFICATION — match the Review Queue page's visual style 
(navy header #1F3864 / bg-slate-800, same DataTable component, 
same button styles) but adapted for this read-only history view:

1. PAGE HEADER:
   - Title bar with text "REVIEW HISTORY" in white bold uppercase
   - Same navy/dark header bar style as review-queue/page.tsx top 
     chrome bar

2. SEARCH BAR (below header):
   - Left: Dropdown labeled "Sample / Review Name" — populate 
     options by extracting unique sampleName values from the 
     fetched rows (client-side, since there's no separate samples 
     lookup endpoint for history)
   - Right: Text input "Borrower Name" (plain text search)
   - "Search" button (dark navy/slate-800) — on click, calls 
     getReviewHistory(selectedSampleName, borrowerNameInput) 
     and refreshes the table
   - On initial page load, call getReviewHistory() with no 
     filters to show all finalized reviews

3. SECTION HEADING:
   - "All Finalized Reviews" bold text, left-aligned, below 
     search bar (in a header bar styled like the "Draft Completed" 
     section header in review-queue: bg-[#1F3864] text-white)

4. TABLE (use the shared DataTable component):
   - theadClassName="bg-slate-800 text-white" (same as review-queue)
   - Columns in this exact order:
     SAMPLE / REVIEW NAME | eCIF # | BORROWER NAME / LINESHEET | 
     REVIEWER | EXPOSURE | BANK PD | CAS PD | COMPLETED
   - Column rendering:
     - Sample/Review Name → row.sampleName
     - eCIF # → row.eCifNumber
     - Borrower Name / Linesheet → render borrower name as plain 
       text (text-slate-800 font-medium, NOT a clickable link since 
       this is historical/read-only) followed by the same small 
       document-icon Button used in review-queue/page.tsx for 
       opening the linesheet PDF. Reuse the same ReviewPDFModal 
       component and the same onClick pattern (setPdfRow) — copy 
       this exact behavior from review-queue/page.tsx, just feed 
       it reviewId, eCifNumber and customerName from this row.
     - Reviewer → row.reviewerName
     - Exposure → format row.exposure as currency, e.g. 
       "$" + Number(exposure).toLocaleString()
     - Bank PD → row.bankPD
     - CAS PD → row.casPD
     - Completed → format row.completedDate as M/D/YYYY 
       (or show "-" if null)
   - This table has NO Actions column (no Edit/Delete/Open — 
     fully read-only)

5. ADDITIONAL FEATURES:
   - Show a loading skeleton/spinner while data is being fetched 
     (reuse whatever loading UI pattern review-queue/page.tsx uses)
   - Show "No finalized reviews found" message when the result 
     array is empty
   - Sorting: implement simple client-side column sorting (sortBy/
     sortDir + useMemo sortedRows), same approach as review-queue/
     page.tsx

IMPORTANT RULES:
- This is a READ-ONLY page — no add, edit, or delete functionality
- Do not modify review-queue/page.tsx or any other existing file
- Reuse the same shared components already imported in 
  review-queue/page.tsx (DataTable, Button, ReviewPDFModal, Modal/
  Dialog if used for loading state) instead of creating new ones
- Match exact spacing, font sizes, and color classes used in 
  review-queue/page.tsx

Show me the complete new page.tsx file when done.

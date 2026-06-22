There's a runtime error on the Review History page when clicking 
the Borrower Name.

Error: An unexpected error occurred (HTTP error from getReviewByKeys)
Call stack shows:
- src/services/api/reviews.ts (449:10) - getReviewByKeys
- This is being called from a handleOpen-style function, but it 
  should NOT be called at all on the Review History page.

Read this file: frontend/src/app/review-history/page.tsx

FIND where the Borrower Name is rendered in the table. If it is 
currently a clickable Link or Button that calls handleOpen, 
getReviewByKeys, or any similar function that navigates to a live 
review (like review-queue does), REMOVE that behavior.

FIX: 
- Render the Borrower Name as plain text only: 
  <span className="text-slate-800 font-medium">{row.customerName}</span>
- Do NOT call getReviewByKeys, handleOpen, or any navigation logic 
  for the borrower name on this page — these are finalized/historical 
  reviews, not active ones, so there's no live review to navigate to
- Keep the linesheet icon button working exactly as it currently 
  does (opening ReviewPDFModal) — that should remain functional

IMPORTANT RULES:
- Do not modify review-queue/page.tsx or reviews.ts
- Only fix review-history/page.tsx
- Do not change any other functionality (search, sort, pagination, 
  linesheet modal)

Show me exactly which lines changed.

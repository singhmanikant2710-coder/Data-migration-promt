Files involved: 
- frontend/src/app/review-status/page.tsx (needs fixing)
- frontend/src/app/review-history/page.tsx (reference — working correctly)

READ-ONLY DIAGNOSTIC. Do NOT edit anything. Report in plain text:

In review-history/page.tsx (the WORKING reference), for the "Completed Draft 
Reviews" / borrower table columns:
1. The pdf/document icon button — paste its exact onClick handler and what it 
   does (it should open a PDF modal like ReviewPDFModal, with setPdfRow or 
   similar state). Include how the modal is rendered.
2. The borrower name — paste its exact onClick handler (the router.push(...) 
   to the review form / review-info page, including the full URL/query params 
   built).

In review-status/page.tsx (the BROKEN one), for its "Completed Draft Reviews 
for Approval" table:
3. The pdf/document icon — paste its current onClick. Where does it currently 
   navigate (it's wrongly going to review-queue)?
4. The borrower name cell — does it have any onClick at all currently? Paste it 
   or say "no handler".
5. Does review-status/page.tsx already import/render a PDF modal component 
   (ReviewPDFModal), or would it need to be added?

Report findings only. No edits.

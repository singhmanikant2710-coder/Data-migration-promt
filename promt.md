Do NOT edit anything. This is about the REVIEW STATUS screen only 
(frontend route /review-status). Ignore review-queue and 
review-history for now.

Report:

A) FRONTEND
1. Find the page file for the /review-status route (likely 
   frontend/src/app/review-status/page.tsx). Confirm the exact path.
2. Show what data it fetches and from which service/API for:
   (a) the 7 summary boxes: Borrowers Sampled, Finalized, 
       Distributed, Approved, Draft Completed, In Progress, 
       Unopened/Cancelled
   (b) the "Completed Draft Reviews for Approval" table
3. Show the JSX + Tailwind classes for the 7 summary boxes — the 
   outer container (grid/flex) and one box — so I can see the 
   alignment structure.
4. Show the service file(s) it imports (frontend/src/services/api/...).

B) BACKEND
5. Search Casrr.Api Controllers and Casrr.Application Services for 
   any endpoint that returns these summary counts and the 
   draft-reviews list for a sample. 
   - If it exists: show the controller route + the SQL behind it.
   - If it does NOT exist: state that clearly.

Report only. No edits anywhere. Do not read review-queue or 
review-history files.

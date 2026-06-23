Do NOT edit anything. I'm working on the Review Status screen 
(frontend route /review-status). I need to fully understand the 
current implementation first.

Report all of the following:

A) FRONTEND
1. Find the page file for /review-status (likely 
   frontend/src/app/review-status/page.tsx). Show what API/service 
   functions it calls for:
   (a) the 7 summary box counts (Borrowers Sampled, Finalized, 
       Distributed, Approved, Draft Completed, In Progress, 
       Unopened/Cancelled)
   (b) the "Completed Draft Reviews for Approval" table rows
2. Show the JSX + CSS/Tailwind classes for the 7 summary boxes 
   (the container and a single box) so I can see the alignment.
3. Show the service file(s) it imports from (e.g. 
   frontend/src/services/api/...).

B) BACKEND
4. Does a backend endpoint already exist for these summary counts 
   and the draft-reviews list? Search Casrr.Api Controllers and 
   Casrr.Application Services. 
   - If yes: show the controller route(s) + the SQL behind them.
   - If no: state clearly that it's missing.

Report only. No edits anywhere.

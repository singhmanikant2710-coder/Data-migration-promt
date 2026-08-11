READ-ONLY. Read these SPECIFIC files ONCE each and show the unlock logic. 
Do NOT search the whole codebase. Do NOT re-read. Just open these 4 files 
and show the relevant parts:

1. backend/src/Casrr.Infrastructure/SqlServer/SqlReviewStatusRepository.cs
   Show any method handling "Unlock" — the field updates for 
   Review_approval_date, Review_initial_approval_date, Review_finalized_date, 
   Review_approver_name, Locked. Show how General Revisions vs Reconsideration 
   vs Appeal are handled.

2. backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs
   Show any Unlock-related method (same fields) IF the unlock logic is here 
   instead.

3. frontend/src/app/review/[ecif]/review-info/components/TopChromeBar.tsx
   Show the Unlock Review button + modal (the 3 options: General Revisions, 
   Reconsideration, Appeal) and any default selection.

4. frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx
   Show the Unlock modal / options IF it's here instead of TopChromeBar.

For each file: show ONLY the unlock-related code (handler, field updates, 
modal options, default). If a file has nothing unlock-related, just say 
"nothing unlock-related here" and move on. Do NOT re-read any file. Findings 
only.

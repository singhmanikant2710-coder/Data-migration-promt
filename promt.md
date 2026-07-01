READ-ONLY. Do NOT edit. Report only.

I want the Delete (trash) icon on a CRM finding row to delete that specific finding 
from dbo.[02_CORE_07_Findings] immediately via an API call, using Review_id + 
Finding_code (which are unique together).

Report ONLY:

1. In CrmFindingsAndRatingsSection.tsx, show the current trash/delete icon handler. 
   Does it call deleteRow (local state only) or any API? In that handler's scope, 
   are BOTH the reviewId and the row's findingCode available?

2. In the frontend API service (e.g. reviews.ts), show how an existing call like 
   saveReview is defined, so I can add a deleteCrmFinding(reviewId, findingCode) 
   in the same style.

3. In backend ReviewController.cs, show one existing endpoint definition (route + 
   method signature) so I can add a delete endpoint in the same style.

4. Show how IReviewRepository declares a method and how SqlReviewRepository 
   implements a simple write, so I can add DeleteCrmFindingAsync(reviewId, findingCode) 
   matching the pattern.

Report only with exact code and file paths. No edits.

#249 – Fixed. Tested in Dev and pushed to the develop branch. Not yet deployed to QA.

What was wrong: Unlocking a review always cleared its Finalized date. When the review was then re-approved, nothing required a new Finalized date, so the review dropped out of Review History even though its sample was Closed.

What changed (your Option 3):
- When a review in a Closed sample is unlocked, the Distributed and Finalized dates are now kept and shown greyed out. Only Mgr Approval is cleared.
- The Mgr Approval date must be on or before the Finalized date. A hint under Mgr Approval shows the required date, and a later date is rejected with a clear message.
- The server enforces these rules as well, so the dates cannot be changed or cleared by any other route.
- Reviews in Open samples behave exactly as before.

What you may notice: After unlocking a review in a Closed sample, it stays in Review History with an empty Mgr Approval until it is re-approved.

Existing orphaned reviews: This fix prevents new cases. Reviews already affected still have no Finalized date, and their original dates were never stored. I will share a list of these for review, and each Finalized date will need to be confirmed by the business before the DBA team corrects them.

I will confirm here once deployed to QA.

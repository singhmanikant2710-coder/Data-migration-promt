Single-file edit: frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx

In the "General Revisions" unlock flow, the finalized date is not being 
cleared (it should be). Add finalizedDate: "" to the save data object so 
[Review_finalized_date] is cleared on unlock.

Find the General Revisions unlock handler (when unlockReason === "GENERAL"). 
It currently builds:
    const data: Record<string, any> = { approvalDate: "" };
    if (!hasInitial && mgr) {
      data.initialApproval = mgr;
    }

Add finalizedDate clearing to that data object:
    const data: Record<string, any> = { approvalDate: "", finalizedDate: "" };
    if (!hasInitial && mgr) {
      data.initialApproval = mgr;
    }

So the General Revisions unlock now: transfers approval->initial (first unlock 
only, unchanged), clears approvalDate (unchanged), AND clears finalizedDate 
(new). The backend SaveReviewTimelineAsync maps finalizedDate to 
[Review finalized date].

CONSTRAINTS:
- ONLY add finalizedDate: "" to the General Revisions data object.
- Do NOT change the approvalDate/initialApproval logic or anything else.
- Do NOT touch Reconsideration/Appeal yet (separate fix).
- Only edit this one file. Show the updated data object in the General 
  Revisions handler.

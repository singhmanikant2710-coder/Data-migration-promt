READ-ONLY. Diagnostics only. Do not change anything.

Investigating the "Unlock Review" workflow (Review Form / Review Status). 
Two bugs + one default-selection enhancement.

Find and show the Unlock Review workflow code — likely in the backend 
(Application/Infrastructure) handling the unlock action, and possibly the 
frontend modal. Show (no edits):

1. The UNLOCK handler/service method that processes "Unlock for General 
   Revisions". Show the full logic — which fields it updates:
   - Does it transfer [Review_approval_date] -> [Review_initial_approval_date] 
     (first unlock only)? (reported working)
   - Does it clear [Review_approval_date]? (working)
   - Does it leave [Review_approver_name]? (working)
   - Does it set [Locked] = FALSE? (working)
   - Does it clear [Review_finalized_date]? ** REPORTED NOT WORKING ** — show 
     whether this field is cleared in the code. Is it missing, or set wrong?

2. The three unlock options: "General Revisions", "Reconsideration", "Appeal". 
   Show how each is handled. Do Reconsideration and Appeal:
   - open their sub-forms (working), THEN
   - follow the SAME field-update workflow as General Revisions? 
   ** REPORTED NOT WORKING for Reconsideration/Appeal ** — show whether they 
   call the same update logic or a different/incomplete path. Is the shared 
   workflow (transfer approval_date, clear approval_date, clear finalized_date, 
   Locked=false) applied to all three, or only to General Revisions?

3. The field mapping: confirm the exact column names — [Review_approval_date], 
   [Review_initial_approval_date], [Review_finalized_date], [Review_approver_name], 
   [Locked] — and how they're set in the unlock update (SQL/EF).

4. The frontend Unlock modal: show the three options and whether a DEFAULT 
   selection is set. Client wants "Unlock for General Revisions" as the default 
   (most common). Show the current default (if any).

5. "First unlock only" logic for the approval_date transfer: how does it 
   detect first unlock (e.g. Review_initial_approval_date IS NULL check)? 
   Confirm this so the same guard applies to Reconsideration/Appeal.

Do not edit anything. Show the unlock handler(s), which fields each of the 
three options updates, the finalized_date handling, and the modal default. 
Findings only.

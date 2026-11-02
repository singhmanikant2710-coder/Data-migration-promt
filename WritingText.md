#238 – Implemented. Pushed to the develop branch. Not yet deployed to QA.
What changed: The email window on the Review Form now has a Bcc line, pre-filled with the reviewer's email (CRO_email) and the manager's email (CRO_manager_email) from the review. Blank addresses and anyone already in To or CC are skipped automatically. The Bcc line can be edited or cleared before sending, and invalid addresses are flagged. Bcc recipients are not visible to the To/CC recipients.
What you may notice: Reviewers and managers will now receive a copy of memo emails sent from CASRR.
Testing: Workflow tested in Dev; final email-delivery check is in progress. I will confirm here once deployed to QA.

#239 – Implemented. Pushed to the develop branch. Not yet deployed to QA.
What changed: When CRO Complete is entered for the first time and the reviewer clicks Save, a Yes/No prompt asks whether to save and submit the review for approval.
- Yes: the review is saved and an email window opens addressed to the manager, with Subject "Draft Review Complete: [Review ID] - [Customer name]" and Body "[Customer name] ([Review ID] - [Sample name]) is complete and awaiting your review." The reviewer is copied in Bcc. The email is only sent when the reviewer clicks Send.
- No: nothing is saved and the reviewer can keep editing. The prompt appears again on the next Save. After a second No, the review saves normally without the prompt, and the reviewer notifies the manager outside CASRR.
- If the save fails, no email window opens. If the manager's email is missing, the window opens with a message to enter the address.
Testing: Workflow tested in Dev; final email-delivery check is in progress. I will confirm here once deployed to QA.

#240 – Implemented. Pushed to the develop branch. Not yet deployed to QA.
What changed: Same workflow as #239 for Mgr Approval. When Mgr Approval is entered for the first time and the manager clicks Save, a Yes/No prompt asks whether to save and notify the reviewer.
- Yes: the review is saved and an email window opens addressed to the reviewer, with Subject "Review Approved: [Review ID] - [Customer name]" and Body "[Customer name] ([Review ID] - [Sample name]) is approved for distribution." The manager is copied in Bcc.
- No: same as #239 (prompt again on the next Save; after a second No, saves normally).
- Re-entering Mgr Approval after a review is unlocked also shows the prompt. All existing approval rules (including the Finalized date rule from #249) still apply; if the save is rejected, no email window opens.
Testing: Workflow tested in Dev; final email-delivery check is in progress. I will confirm here once deployed to QA.


git add backend/src/Casrr.Api/Controllers/AdminController.cs backend/src/Casrr.Application/Services/SmtpEmailService.cs frontend/src/services/api/admin.ts "frontend/src/app/review/[ecif]/review-info/page.tsx" "frontend/src/app/review/[ecif]/review-info/types.ts" "frontend/src/app/review/[ecif]/review-info/hooks/useSaveAndNotify.ts"
git status
git commit -m "UAT #238, #239, #240: Bcc in Review Form email (CRO and manager pre-filled); save-and-notify prompts for CRO Complete and Mgr Approval open a pre-filled notification email"
git push origin develop

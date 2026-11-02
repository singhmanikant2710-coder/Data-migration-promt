UAT #238, #239, #240 – Review Form email enhancements (Bcc + CRO Complete and Mgr Approval notifications)

CONTEXT
- One Email modal on the Review Form: ShareEmailDialog in frontend/src/app/review/[ecif]/review-info/page.tsx, opened from TopChromeBar.tsx. It sends via frontend/src/services/api/admin.ts → AdminController.cs (send-email) → SmtpEmailService.cs.
- #253 (just done): up to 5 PDF attachments, server-side validation, sanitised file names, no raw errors. This must keep working exactly as now.
- Related save rules that must not change: #212 (Distributed/Finalized date rules), #249 (Closed sample: Mgr Approval must be on or before Finalized, server-enforced), #233 (save sends only edited fields).
- Existing Send button needs a Document Type (Initial Memo / Final Memo).

ENHANCEMENTS
#238 – Add a Bcc line to the email modal, pre-filled from the review: CRO_email and CRO_manager_email (dbo.[02_CORE_02_Reviews]). Purpose: sender and manager get a copy of memo distribution emails.
#239 – When the reviewer first enters CRO Complete and saves: Yes/No modal "Save your work and submit the review for approval?". Yes → save, then open the email modal pre-filled: To = CRO_manager_email; Subject = "Draft Review Complete: [Review_ID] - [Customer_name]"; Body = "[Customer_name] ([Review_ID] - [Sample_name]) is complete and awaiting your review." No → close, keep editing; on the next Save run the prompt again. After a second No, the CRO notifies the manager outside CASRR.
#240 – Same workflow when a manager first enters Mgr Approval and saves: "Save your work and notify the CRO of the approval?". Yes → save, then email modal: To = CRO_email; Subject = "Review Approved: [Review_ID] - [Customer_name]"; Body = "[Customer_name] ([Review_ID] - [Sample_name]) is approved for distribution." No → same as #239.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Email path end to end with file:line: modal fields/state, validation, Send disabled condition, service payload, controller binding, SMTP send. Does the SMTP service support Bcc today?
- Where CRO_email, CRO_manager_email, Customer_name, Review_ID and Sample_name are available on the Review Form (already loaded? from which API/DTO?). Exact column names in the DB.
- Save flow with file:line: where CRO Complete and Mgr Approval are edited (ReviewInfoSection.tsx), how the main Save and section saves work, how a save success/failure is reported, and where a "field changed from empty to a value" can be detected reliably (stored value vs edited value).
- How unlock/re-approval (#249) and the #212/#249 server rules interact with saving Mgr Approval.
- Who can set CRO Complete / Mgr Approval (role checks, browser and server).
- Every other caller of the email modal, sendEmail and SmtpEmailService (must not break).
- List anything in the requirements that is ambiguous for the code.

PHASE 2 – PLAN (smallest generic change)
- #238: Bcc field (same multi-address format as CC), pre-filled from the review, editable; skip blank/duplicate addresses and addresses already in To/CC; server validates every Bcc address like To/CC; SMTP sends Bcc (not visible to To/CC recipients).
- #239/#240: one shared, reusable "save and notify" workflow driven by config (trigger field, modal text, To, subject, body), not two copies of the code.
  - Trigger only when the field goes from empty (stored) to a value in this edit session.
  - Save first; open the email modal only if the save succeeded. If the server rejects the save (e.g. #249 rule), show the existing error and no email modal.
  - No → close and keep the user's edits; prompt again on the next Save.
  - The email is never sent automatically: the user reviews and clicks Send in the existing modal.
  - Subject/body placeholders with a NULL/blank Customer_name, Sample_name or email must not show "null"/"undefined".
- Do NOT decide these silently – list options with a recommendation for me:
  1. Notification emails have no memo: should Document Type and attachments be optional for them (and stay required for normal memo emails)?
  2. Should the #238 Bcc pre-fill also apply to the #239/#240 notification emails?
  3. "Prompt again after No": every Save until Yes, at most twice per session, or once per page load?
  4. Re-entering Mgr Approval after an unlock (#249): treat as "first time" and prompt again?
  5. CRO_manager_email / CRO_email blank: open the modal with an empty To, or skip the email step with a message?
- Existing behaviour must stay identical: normal Save when these fields are not being set, memo emails, #253 attachments, #212/#249/#233 rules, other tabs.
- No schema change expected. SQL only as idempotent scripts/sql/ if truly needed – ask first.

PHASE 3 – IMPLEMENT (only after I answer the Phase 2 decisions)
- Touch only the needed code. Reuse the existing modal, toast and save helpers.

PHASE 4 – VERIFY + REPORT (in this chat, do not write the report to a file)
- Per enhancement: what was needed (file:line), ADDED (file:line), REMOVED/CHANGED (file:line).
- On-screen before/after table for: memo email with Bcc pre-filled; editing/removing Bcc; CRO Complete first save → Yes; → No then Save again; Mgr Approval first save → Yes; → No; save rejected by #249 rule; saving other fields without touching these dates; editing an already-filled CRO Complete/Mgr Approval date.
- NULL/edge cases: blank/NULL CRO_email or CRO_manager_email; same address in To and Bcc; invalid Bcc address; NULL Customer_name/Sample_name; network error during save; user closes the email modal without sending.
- Confirm other screens unaffected (other Email callers, #253 attachments, Review Queue/History, other tabs).
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave untracked scripts/sql/find-reviews-with-blanked-ratings.sql alone).
- Do not commit or push.

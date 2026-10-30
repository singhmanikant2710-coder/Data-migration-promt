#232 – Concurrent save protection
Scope: Detect when a review was updated by another user after it was opened and block the overwrite. Add a row version to the Reviews record (idempotent DBA script), return it when the review loads, send it with every save, and have the server reject a save whose version is outdated (HTTP 409). The user sees: "This review was updated by another user. Please refresh to see the latest changes before saving." No change to normal day-to-day saving. Edit-mode locking (option 2) is out of scope.

#234 – Covenant Type searchable dropdown
Scope: Add a type-to-filter search box at the top of the Covenant Type dropdown in the Performance Covenants and Monitoring Covenants sections. Partial text filters the list (case-insensitive); keyboard selection supported. Built as a reusable searchable dropdown component. Saved values and the list source are unchanged.

#236 – Policy Exception Description searchable dropdown
Scope: Reuse the searchable dropdown from #234 for the Policy Exception Description field (52 options). Partial text filters the list. Existing rules are kept: duplicate types stay blocked, and notes/mitigation behaviour is unchanged.

#238 – Bcc in Email
Scope: Add a Bcc field to the Review Form email modal, pre-filled with CRO_email and CRO_manager_email from the review (blank/duplicate addresses skipped, user can edit). Extend the email API and SMTP service to send Bcc recipients, with server-side email validation. To/CC, attachments and Document Type behaviour are unchanged.

#239 – CRO Complete notification
Scope: When CRO Complete is entered and saved for the first time, show a Yes/No modal asking to save and submit for approval. Yes: save the review, then open the email modal pre-filled – To: CRO_manager_email; Subject: "Draft Review Complete: [Review_ID] - [Customer_name]"; Body: "[Customer_name] ([Review_ID] - [Sample_name]) is complete and awaiting your review." No: close the modal and continue editing; the prompt shows again on the next Save. Normal save is unchanged when CRO Complete is not being set.

#240 – Mgr Approval notification
Scope: When Mgr Approval is entered and saved for the first time, show a Yes/No modal asking to save and notify the CRO. Yes: save the review, then open the email modal pre-filled – To: CRO_email; Subject: "Review Approved: [Review_ID] - [Customer_name]"; Body: "[Customer_name] ([Review_ID] - [Sample_name]) is approved for distribution." No: close the modal and continue editing; the prompt shows again on the next Save. Existing approval rules (#212, #249 Finalized-date check) are unchanged.

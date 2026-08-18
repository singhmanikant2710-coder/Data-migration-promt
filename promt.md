READ-ONLY. Read once. Do not re-read.

File: frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx

Bug #180(d) reopened: General Revisions unlock works, but Reconsideration and 
Appeal unlock are NOT (a) unlocking the review (Locked stays true), and (b) 
hiding the Unlock button afterward. We previously added unlock fields to their 
submit payloads. Verify the current state.

Show:
1. The RECONSIDERATION submit handler — the full saveReview data object. Does it 
   currently include approvalDate: "", finalizedDate: "", and the first-unlock 
   initialApproval? Show the exact current data object.
2. The APPEAL submit handler — same, show its full current data object.
3. Compare with the GENERAL REVISIONS handler (which works) — show its data 
   object. What does General Revisions do that Reconsideration/Appeal don't? 
   Specifically, how does the review get UNLOCKED (Locked -> false)? Is there a 
   "locked: false" field, or does clearing approvalDate trigger the unlock in 
   the backend?
4. UNLOCK BUTTON visibility condition: show the exact condition that shows/hides 
   the Unlock button (reviewLocked / canUnlock / approval date). What must be 
   cleared for the button to hide? Does General Revisions clear that but 
   Reconsideration/Appeal don't?
5. Do Reconsideration/Appeal actually SEND the unlock fields on submit, or is 
   there a separate code path where their submit only saves the sub-form fields 
   (reconsideration/appeal details) and skips the unlock fields? Show the exact 
   submit call for each.

Do NOT edit. Show all three handlers' data objects, how unlock (Locked->false) 
actually happens for General Revisions, the button visibility condition, and 
whether Reconsideration/Appeal send the unlock fields. Findings only.

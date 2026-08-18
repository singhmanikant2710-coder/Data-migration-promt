Single-file edit: frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx

Bug #180(d): Reconsideration and Appeal unlock don't unlock the review / hide 
the Unlock button, while General Revisions does. Two root causes found:

ROOT CAUSE 1: General Revisions calls setIsApprovedFromQueue(false) after save 
(which hides the Unlock button immediately). Reconsideration and Appeal do NOT 
call it, so reviewLocked stays true and the button remains visible.

ROOT CAUSE 2: The initialApproval spread condition in Reconsideration/Appeal is 
different (and wrong) compared to General Revisions:
- General (correct): (!hasInitial && mgr) where hasInitial = 
  !!String(s?.initialApproval).trim()  → "initialApproval is blank AND mgr 
  exists → set it"
- Reconsideration/Appeal (wrong): 
  !(String(s?.initialApproval ?? "").trim() && toInputDateString(s?.mgrApproval ?? ""))
  → this is a different/incorrect condition.

FIX BOTH the Reconsideration submit and the Appeal submit:

FIX A — Correct the initialApproval condition to MATCH General Revisions. 
Replace the spread condition:
    ...(!(String(s?.initialApproval ?? "").trim() && toInputDateString(s?.mgrApproval ?? "")) ? { initialApproval: toInputDateString(s?.mgrApproval ?? "") } : {})
with the same logic General Revisions uses:
    ...(!String(s?.initialApproval ?? "").trim() && toInputDateString(s?.mgrApproval ?? "") ? { initialApproval: toInputDateString(s?.mgrApproval ?? "") } : {})
(The difference: General is "!initialApproval && mgr" — set only when 
initialApproval is blank AND mgr exists. Apply this exact condition to both 
Reconsideration and Appeal.)

FIX B — After the saveReview call succeeds in BOTH the Reconsideration and 
Appeal submit handlers, add the same post-save unlock reflection that General 
Revisions does:
    setIsApprovedFromQueue(false);
    reloadPageData();
Add these right after the successful saveReview in both handlers (if 
reloadPageData is already called, just add setIsApprovedFromQueue(false) before 
it). This makes reviewLocked go false immediately and hides the Unlock button, 
matching General Revisions.

CONSTRAINTS:
- Apply FIX A (correct initialApproval condition) and FIX B 
  (setIsApprovedFromQueue(false) + reloadPageData) to BOTH the Reconsideration 
  and Appeal submit handlers.
- Do NOT change the sub-form fields (reconsideration*/appeal* data), 
  approvalDate/finalizedDate (already correct), or General Revisions.
- Only edit this one file. Show the updated Reconsideration and Appeal handlers 
  (the corrected condition + the added setIsApprovedFromQueue(false)/
  reloadPageData).

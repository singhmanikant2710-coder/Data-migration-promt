Single-file edit: frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx

The Reconsideration and Appeal unlock flows open their sub-forms and save 
their own fields, but they do NOT apply the same unlock workflow as General 
Revisions (transfer approval->initial on first unlock, clear approvalDate, 
clear finalizedDate). Client wants all three unlock options to follow the SAME 
core unlock workflow. Add that workflow to both the Reconsideration submit and 
the Appeal submit save payloads.

The General Revisions unlock does this (reference):
    const hasInitial = !!String((...initialApproval ?? "")).trim();
    const mgr = toInputDateString((...mgrApproval ?? ""));
    const data = { approvalDate: "", finalizedDate: "" };
    if (!hasInitial && mgr) { data.initialApproval = mgr; }

Apply the SAME core unlock fields to BOTH the Reconsideration submit and the 
Appeal submit save payloads, MERGED with their existing sub-form fields.

RECONSIDERATION submit — currently saves:
    data: { reconsideration, reconsiderationDate, reconsiderationDescription, 
            reconsiderationDecision, reconsiderationRationale, appeal, 
            appealDate, appealDescription, appealDecision, 
            appealDecisionRationale }
ADD the unlock fields to this same data object (do not remove the existing 
reconsideration/appeal fields):
    - approvalDate: ""
    - finalizedDate: ""
    - and, first-unlock-only: if initialApproval is not already set and mgr 
      approval exists, add initialApproval: mgr
   Use the SAME hasInitial/mgr computation as General Revisions.

APPEAL submit — currently saves:
    data: { appeal, appealDate, appealDescription, appealDecision, 
            appealDecisionRationale, reconsideration, ... }
ADD the same unlock fields to this data object too:
    - approvalDate: ""
    - finalizedDate: ""
    - first-unlock-only initialApproval: mgr (same guard)

So Reconsideration and Appeal now BOTH: save their sub-form fields AND apply 
the unlock workflow (transfer approval->initial first time, clear approval, 
clear finalized) — identical to General Revisions. The [Locked] flag handling 
(via backend/approver-lock) stays as-is; only these date fields are added.

CONSTRAINTS:
- Add approvalDate: "", finalizedDate: "", and the first-unlock-only 
  initialApproval to BOTH the Reconsideration and Appeal submit data objects.
- Compute hasInitial and mgr the SAME way General Revisions does (reuse the 
  same source values).
- Do NOT remove or change the existing reconsideration/appeal sub-form fields.
- Do NOT change General Revisions (already done) or anything else.
- Only edit this one file. Show the updated Reconsideration and Appeal submit 
  data objects with the merged unlock fields.

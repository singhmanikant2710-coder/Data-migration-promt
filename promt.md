READ-ONLY. Read ONE file once. Do not search or re-read.

File: frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx

I need to confirm the exact field name used in the saveReview data object for 
the FINALIZED date, so I can add it to the General Revisions unlock. Show:

1. Any place in this file where a "finalized" date field is referenced in a 
   saveReview/reviewInfo data object or read from response (e.g. finalizedDate, 
   reviewFinalizedDate, mgrFinalized, etc.) — the EXACT property name.
2. The saveReview data object shape — show existing fields like approvalDate, 
   initialApproval, and confirm what the finalized-date property is called in 
   this frontend layer (to match backend [Review finalized date]).
3. Also show where mgrApproval / initialApproval are read from (the response 
   object) so I know the source object for finalized date too.

Do NOT edit. Just confirm the exact finalized-date property name used in this 
file's save/data mapping. Findings only.

There's a runtime error on the Review Queue page when opening a review.
Backend logs show GET /review-queue returns 200 OK fine — so the list 
loads. The error happens inside handleOpen when calling getReviewByKeys.

Error: [browser] Failed to open review Error: An unexpected error occurred
- at http (src/lib/api.ts:156:21)
- at async getReviewByKeys (src/services/api/reviews.ts:449:10)
- at async handleOpen (src/app/review-queue/page.tsx:221:19)

Read these files completely. Do NOT modify anything yet:
1. frontend/src/app/review-queue/page.tsx — focus on handleOpen 
   (around line 221) and the row click handler that calls it
2. frontend/src/services/api/reviews.ts — focus on getReviewByKeys 
   (around line 449): what URL/endpoint does it call and what query 
   params does it build?

Report back:
1. What exact values (reviewId, sampleId, ecif) does handleOpen 
   read from the clicked row, and does it pass them to getReviewByKeys?
2. What is the exact endpoint URL getReviewByKeys constructs?
3. Is there any guard for when reviewId / sampleId / ecif is null, 
   undefined, or empty string before the call is made?
4. Around line 247 of page.tsx there's a comment about "duplicate ID" 
   — what is that code block doing on error?

Do NOT edit anything. Just report these findings.

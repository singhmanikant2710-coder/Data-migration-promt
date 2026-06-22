Good, the frontend guarding is fine. The error is the backend 
endpoint GET /api/v1/reviews/review returning a non-2xx status, 
which makes api.ts throw.

I need to see the EXACT request and backend response. Do NOT 
modify any code.

1. In frontend/src/app/review-queue/page.tsx, inside handleOpen 
   (around line 221), add a temporary console.log RIGHT BEFORE 
   the getReviewByKeys call that prints the exact reviewId, 
   sampleId, and ecif values being passed. Use:
   console.log("OPEN REVIEW PARAMS:", { reviewId, sampleId, ecif });
   This is the ONLY change. Do not touch anything else.

2. Then tell me: what is the full backend endpoint signature for 
   GET /api/v1/reviews/review? Find the controller action in 
   backend/src/Casrr.Api/Controllers/ReviewController.cs that 
   handles this route. What [FromQuery] parameters does it expect, 
   and what does it return / what status codes can it produce 
   (e.g. NotFound, BadRequest)?

Report the controller action code and the expected query parameters.

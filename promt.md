Modify ONLY this file:
frontend/src/services/api/reviews.ts

Add a new API function to delete one CRM finding, matching the style of saveReview. 
The backend endpoint is:
   DELETE /api/v1/reviews/crm-finding?reviewId={id}&findingCode={code}

Add:

export async function deleteCrmFinding(
  reviewId: number,
  findingCode: string
): Promise<void> {
  await del(
    `/api/v1/reviews/crm-finding?reviewId=${reviewId}&findingCode=${encodeURIComponent(findingCode)}`
  );
}

If the api lib (@/lib/api) does not export a "del" (DELETE) helper, use whatever 
DELETE method it exposes (show me what's available). If only get/post exist and 
there is no delete helper, STOP and tell me what the api lib exports so I can 
choose the right approach.

Modify ONLY reviews.ts.

Frontend only. Two files. Do NOT modify the backend. Do NOT change any other field. Do not plan. Just apply.

UAT #159: The backend now returns cancelled, cancelledDate and cancellationRationale on the review-info section. Surface the Cancellation Rationale on the Review Form so users can see why a review was cancelled.

FILE 1: frontend/src/services/api/reviews.ts
In the ReviewInfoSection type, ADD three optional fields at the end (do not change any existing field):
    cancelled?: boolean | null;
    cancelledDate?: string | null;
    cancellationRationale?: string | null;

FILE 2: frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx
Add a read-only "CANCELLATION RATIONALE" field to the Review Status panel, rendered ONLY when the review is cancelled (i.e. when cancelled is true or cancellationRationale is non-empty). Follow the exact styling pattern of the existing read-only fields on that panel (e.g. the disabled/readOnly input used for MGR APPROVAL when the user can't edit it, or the <Field> component used in view mode).

Place it after the existing Review Status fields. Also show CANCELLED DATE next to it using the same date-display formatting as the other date fields on that panel.

Both fields must be read-only — no editing, no save wiring.

Confirm the exact paths to these values on the hook's data object before applying (check useReviewInfo.ts — if cancellationRationale / cancelled / cancelledDate are not mapped through, add them to the hook's mapping following the same pattern used for the other reviewInfo fields, and report that as a third file changed).

Run read-only TypeScript diagnostics on the changed files only.

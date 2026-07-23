Frontend only. Two files. Do NOT modify any other file. Do NOT change the backend. Do not plan. Just apply.

UAT #127 (EIC part): The backend now returns `examinerInCharge` on the review-info section, sourced from dbo.[02_CORE_01_Samples].[EIC_Name]. The UI currently derives Examiner in Charge from the CRO/Reviewer name, which is wrong. Bind it to the new backend field instead.

FILE 1: frontend/src/services/api/reviews.ts
In the ReviewInfoSection type, ADD one optional field directly after managerEmail (do not change any existing field):
    examinerInCharge?: string | null;

FILE 2: frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useReviewInfo.ts
In mapReviewToReviewInfo, replace the current examinerInCharge derivation (which currently falls back to reviewerName / ci.executiveCreditOfficer) with a direct bind to the backend field:
    const examinerInCharge = (review.examinerInCharge as string | null | undefined) ?? "";

Remove the old fallback chain and its comment entirely. Do NOT change reviewerName, managerName, reviewerEmailRaw, managerEmailRaw, cleanEmail(), the Dates section, or anything else in this file or function.

Run read-only TypeScript diagnostics on both files only, and report the changed lines.

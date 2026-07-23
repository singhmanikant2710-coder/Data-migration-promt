Frontend only. Single file: frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useReviewInfo.ts
Do NOT modify any other file. Do NOT change the backend or the API types. Do not plan. Just apply.

UAT #127 (point 1): In mapReviewToReviewInfo, the Assignments fields fall back to Customer Info values when the CRO fields are null, causing Review 21882 (CRO_name NULL) to display the Relationship Manager name in "Examiner in Charge" and the Portfolio Manager name in "Manager". These fields must be blank when the corresponding CRO fields are null.

Change the three mappings as follows — remove ALL fallbacks to Customer Info:

1) reviewerName — currently:
     const reviewerName =
       (review.reviewerName as string | null | undefined) ??
       (ci.reviewerName as string | null | undefined) ??
       (ci.relationshipManager as string | null | undefined) ??
       (ci.primaryRelationshipManager as string | null | undefined) ??
       "";
   Change to bind only to the CRO name from the review:
     const reviewerName = (review.reviewerName as string | null | undefined) ?? "";

2) managerName — currently:
     const managerName =
       (review.managerName as string | null | undefined) ??
       (ci.managerName as string | null | undefined) ??
       (ci.portfolioManager as string | null | undefined) ??
       "";
   Change to:
     const managerName = (review.managerName as string | null | undefined) ?? "";

3) examinerInCharge — currently falls back to the Executive Credit Officer:
     const examinerInCharge =
       (reviewerName as string | null | undefined) ??
       (ci.executiveCreditOfficer as string | null | undefined) ??
       "";
   Change to:
     const examinerInCharge = reviewerName;

Do NOT change reviewerEmail / managerEmail unless they use the same Customer Info fallback pattern — if they do, apply the same rule (bind only to the review's CRO email fields, blank otherwise) and report it.

Do NOT touch any other mapping in this hook, and do NOT change any other section.

Run read-only TypeScript diagnostics on this file only.

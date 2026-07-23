Frontend only. Single file: frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useReviewInfo.ts
Function: mapReviewToReviewInfo

STRICT SCOPE — apply ONLY the exact edits listed below. Do NOT touch any other line, variable, mapping, import, or function. Do NOT refactor or reformat anything. Do NOT change examinerInCharge in this step. Do not plan. Just apply.

Context (UAT #127, confirmed by client): when a review is loaded without an assigned CRO, Reviewer Name and Manager must display as blank until assigned later in the Review Form. They currently fall back to the Customer Info Relationship Manager / Portfolio Manager, which is incorrect.

EDIT 1 — reviewerName. Replace:
    const reviewerName =
      (review.reviewerName as string | null | undefined) ??
      (ci.reviewerName as string | null | undefined) ??
      (ci.relationshipManager as string | null | undefined) ??
      (ci.primaryRelationshipManager as string | null | undefined) ??
      "";
With:
    const reviewerName = (review.reviewerName as string | null | undefined) ?? "";

EDIT 2 — managerName. Replace:
    const managerName =
      (review.managerName as string | null | undefined) ??
      (ci.managerName as string | null | undefined) ??
      (ci.portfolioManager as string | null | undefined) ??
      "";
With:
    const managerName = (review.managerName as string | null | undefined) ?? "";

EDIT 3 — reviewerEmailRaw. Remove ONLY the `(ci.reviewerEmail ...)` fallback so it becomes:
    const reviewerEmailRaw = (review.reviewerEmail as string | null | undefined) ?? "";

EDIT 4 — managerEmailRaw. Remove ONLY the `(ci.managerEmail ...)` fallback so it becomes:
    const managerEmailRaw = (review.managerEmail as string | null | undefined) ?? "";

Leave examinerInCharge, cleanEmail(), the Dates section, and everything else exactly as-is.

After applying, run read-only TypeScript diagnostics on this file only and report the 4 changed lines.

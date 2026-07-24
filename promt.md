Frontend only. Single file: frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useReviewInfo.ts
Do NOT modify any other file. Do NOT change the backend. Do not plan. Just apply.

UAT #148 (item 1): The "Sample Target" field on Review Info renders blank. The API response is correct — for Review 21640 the review-info payload contains "sampleTarget": "Enterprise" — so the bug is in the hook's mapping.

The current mapping is:
    nz(review?.sampleTarget as string | null | undefined) ||
      sampleTarget?.sampleName,

This has two problems: it falls back to `sampleTarget?.sampleName` (which is a sample NAME, not the target), and `sampleTarget` there appears to reference a different object than the string field.

Replace that mapping so sampleTarget binds ONLY to the value returned by the backend:
    sampleTarget: (review?.sampleTarget as string | null | undefined) ?? "",

Remove the `|| sampleTarget?.sampleName` fallback entirely. Do NOT change any other mapping, the nz() helper, or anything else in this file.

After applying, report the exact changed line and run read-only TypeScript diagnostics on this file only.

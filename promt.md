Modify ONLY this file:
backend/src/Casrr.Application/IReviewRepository.cs

Add a declaration for the new SaveCrmFindingsAsync method, matching the style 
of the existing SaveKeyRisksAsync / UpsertChecklistAsync declarations. 
The implementation in SqlReviewRepository.cs has this signature:

    Task SaveCrmFindingsAsync(
        int reviewId,
        IEnumerable<CrmFindingRow>? findings,
        CancellationToken ct);

Add a matching declaration with a short comment like the others 
(e.g. "// Persist CRM Findings into dbo.[02_CORE_07_Findings] (replace-all)").
Ensure CrmFindingRow is imported/available in this file (use the same namespace 
the implementation uses).

Modify ONLY IReviewRepository.cs. If CrmFindingRow is not accessible here and 
needs a using/namespace change in ANOTHER file, STOP and tell me first.

READ-ONLY. Do NOT edit. Report only.

In ReviewService.SaveAsync, I need to add a CRM Findings block after the Checklist 
block. Report ONLY:

1. Show the lines immediately AFTER the Checklist block 
   (if (dto.Checklist?.Length > 0) { ... UpsertChecklistAsync ... }) 
   so I know exactly where to insert the new block.

2. Is there a local TryGetPropertyIgnoreCase helper already in scope in this method 
   that I can reuse, or is it redefined inside each block?

3. Show how resolvedReviewId is obtained earlier in the method.

Report only. No edits.

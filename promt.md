Modify ONLY this file:
backend/src/Casrr.Application/Services/ReviewService.cs

In SaveAsync, find the CRM Findings block that starts with:
    if (dto.CrmFindingsAndRatings is not null && dto.CrmFindingsAndRatings.Change != SectionChangeKind.None)

Right after the opening brace { of that if, BEFORE the try, add this line:
    throw new Exception("CRM_BLOCK_REACHED_TEST");

Modify ONLY ReviewService.cs. Nothing else.

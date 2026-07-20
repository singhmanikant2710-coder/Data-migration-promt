Backend only. Add the two manager number columns to the review GET response so the frontend can display "Number - Name". Use LIVE DB, ignore columns.csv. Single file per edit. Do NOT modify or revert any existing logic authored by anyone (including Jothi) — only ADD. Every existing field must keep behaving exactly as it does.

Context: dbo.[02_CORE_02_Reviews] already stores Relationship_mgr_number (int) and Portfolio_mgr_number (int). The save path writes them. The GET/read path does not return them.

1) Find the repository method that READS the customer-info section for the review form (in backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs — the SELECT that populates the customer info part of the review payload, reading columns like Relationship_mgr_name, Portfolio_mgr_name, ECO_name, SCO_name).
   ADD [Relationship_mgr_number] and [Portfolio_mgr_number] to that SELECT list, and read them as nullable ints (DBNull-safe) into the domain/DTO object. Do not remove or reorder existing columns.

2) Find the domain model / DTO that carries the customer-info read data up to the API contract.
   ADD two nullable properties:
     int? RelationshipManagerNumber
     int? PortfolioManagerNumber
   Do not rename or remove any existing property.

3) Find the API contract returned by the review GET endpoint for the customerInfo section (the one whose fields map to the frontend CustomerInfoSection type: relationshipManager, portfolioManager, executiveCreditOfficer, etc.).
   ADD two nullable properties so they serialise as:
     relationshipManagerNumber
     portfolioManagerNumber
   And map them from the domain/DTO in whatever mapping method already maps relationshipManager and portfolioManager.

Do not touch the frontend in this step. Do not change the save path. Report the files changed and the exact new SELECT column lines.

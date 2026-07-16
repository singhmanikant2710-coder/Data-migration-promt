Read-only. No edits. No plan. Do NOT modify or revert anyone's work (including Jothi's). Just OPEN these files and paste the EXACT relevant code. Do not summarise — paste actual lines.

Context: I need to know whether the RM/PM save path already persists BOTH name and number, or only names, for these dbo.[02_CORE_02_Reviews] columns: Relationship_mgr_name, Relationship_mgr_number, Portfolio_mgr_name, Portfolio_mgr_number.

Open and paste:

1) frontend/src/app/review/[ecif]/review-info/components/sections/CustomerInfoSection.tsx
   - Paste the SelectField component's props/signature usage AND find the SelectField definition file. What does SelectField accept for `options` — a string[] or an array of {value,label} objects? Paste the SelectField definition (frontend/src/components/... — locate it).
   - How does SelectField write to form state (setField/setSection via FormChangesContext)? Does it store a single string, or can it store a value + label separately?

2) frontend/src/services/api/reviews.ts (or wherever CustomerInfoSection type is defined)
   - Paste the type definition for the customerInfo section / relationshipManager / portfolioManager fields. Are there existing fields for the manager NUMBER, or only the name/string?

3) backend/src/Casrr.Application/Reviews/Contracts/ReviewFormSaveModels.cs
   - Paste the exact fields in the customer-info / relationship save model. List every property related to relationshipManager / portfolioManager (name and number).

4) backend/src/Casrr.Application/Services/ReviewService.cs
   - Paste the SaveAsync section that handles customer-info / relationship manager fields, including the postedSections guard around line 72.

5) backend/src/Casrr.Api/Controllers/ReviewController.cs
   - Paste the postedSections guard and any handling of relationship manager fields.

6) The IReviewRepository implementation in backend/src/Casrr.Infrastructure/SqlServer/ (find the SQL that writes to dbo.[02_CORE_02_Reviews])
   - Paste the exact UPDATE/INSERT statement and parameter mapping for Relationship_mgr_name, Relationship_mgr_number, Portfolio_mgr_name, Portfolio_mgr_number. If only name columns are written, say so explicitly.

7) backend/src/Casrr.Api/Controllers/LookupsController.cs + its ReportingService + IReportingRepository implementation
   - Paste ONE complete existing lookup end-to-end (e.g. relationship/segments): controller method, service method, repository method + SQL. This is the template I'll copy for the two new Data Mart lookups.

Paste raw code for each. Change nothing.

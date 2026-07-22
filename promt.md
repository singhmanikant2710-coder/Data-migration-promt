Task: Implement UAT Bug #128 with the MINIMUM SAFE CHANGE based on the completed read-only investigation.

STRICT SAFETY RULES:

- Make ONLY the changes strictly required for Bug #128.
- Do NOT modify, refactor, reformat, rename, or clean up unrelated code.
- Do NOT modify anyone else's work, including Jothi's changes.
- Preserve all existing business logic outside the FHN NAICS Industry lookup data source.
- Do NOT change frontend code unless code evidence proves it is necessary.
- Do NOT create a new endpoint.
- Do NOT change save/edit behavior, DTOs, form bindings, or unrelated lookup behavior.
- Do NOT modify database objects/tables/views/stored procedures.
- Do NOT commit or push.
- Do not overwrite or revert pre-existing local changes.
- Inspect current/latest code before editing.
- Make the smallest possible diff.

==================================================
BUG #128
==================================================

Screen/Tab:
Review Form / Customer Info

Section/Field:
Customer Profile

Field:
FHN NAICS INDUSTRY

UAT Requirement:

The dropdown values must come from a DISTINCT SELECT of:

[01_DATA_01_Data Mart Trial].[IntRepCM].[SubCategory]

The dropdown should contain unique SubCategory values from this required source.

==================================================
READ-ONLY INVESTIGATION FINDINGS
==================================================

The frontend is already correctly wired.

Frontend file:

frontend/src/app/review/[ecif]/review-info/components/sections/CustomerInfoSection.tsx

It already loads:

getLookupOptions("fhn-naics-industries")

which calls:

GET /api/v1/lookups/fhn-naics-industries

The dropdown is already bound to the existing NAICS Industry form field.

Therefore:

DO NOT change CustomerInfoSection.tsx.
DO NOT change frontend dropdown binding.
DO NOT change save/edit behavior.

Existing API endpoint:

backend/src/Casrr.Api/Controllers/LookupsController.cs

GET /api/v1/lookups/{name}

This already delegates lookup resolution through the existing Reporting service.

DO NOT create a new endpoint.

ROOT CAUSE:

The existing backend lookup for:

"fhn-naics-industries"

currently ultimately reads DISTINCT values from:

IntRepCMLSubCategory

This does NOT match Bug #128.

The required source is:

[01_DATA_01_Data Mart Trial].[IntRepCM].[SubCategory]

Therefore the issue is a BACKEND LOOKUP DATA-SOURCE MISMATCH, not a frontend binding issue.

==================================================
REQUIRED IMPLEMENTATION
==================================================

Before changing anything:

1. Trace the existing mapping for:
   "fhn-naics-industries"

2. Confirm which existing repository method it calls.

Likely existing method:

GetDistinctFhnNaicsIndustriesAsync(...)

3. If the service mapping already correctly routes
   "fhn-naics-industries"
   to that repository method:

   DO NOT modify ReportingService.cs.

4. If the repository interface signature does not need to change:

   DO NOT modify IReportingRepository.cs.

5. Modify ONLY the repository SQL implementation necessary to make the existing lookup return DISTINCT SubCategory values from:

[01_DATA_01_Data Mart Trial].[IntRepCM]

Required column:

[SubCategory]

The logical query must follow the requirement:

SELECT DISTINCT [SubCategory]
FROM [01_DATA_01_Data Mart Trial].[IntRepCM]

Preserve existing project conventions for:
- trimming values
- excluding NULL values
- excluding blank/empty values
- ordering, if the existing lookup convention uses deterministic ordering.

IMPORTANT:
Use the project's existing database connection/query conventions.

Do NOT blindly introduce a cross-database reference if the repository connection is already connected to [01_DATA_01_Data Mart Trial].

Inspect the actual connection/query pattern first and use the safest existing convention.

==================================================
CRITICAL SCOPE CONTROL
==================================================

Expected minimal change:

Preferably ONLY:

backend/src/Casrr.Infrastructure/SqlServer/SqlReportingRepository.cs

Do NOT automatically modify 2–3 files just because the investigation estimated 2–3 files.

Only modify ReportingService.cs if the existing
"fhn-naics-industries"
mapping is actually incorrect.

Only modify IReportingRepository.cs if a signature change is genuinely required.

No frontend files should be changed.

No DTO changes should be required.

No controller changes should be required.

No new API endpoint should be created.

No database object changes.

==================================================
DO NOT CHANGE
==================================================

Do not affect any other lookup, including:

- Relationship Segment
- Unit
- Market
- Relationship Managers
- Portfolio Managers
- any other Customer Profile dropdown

Do not change:

- selected value handling
- existing saved NAICS Industry values
- FormChangesContext behavior
- save/update APIs
- CustomerInfoFields DTO
- HasNaicsIndustry logic
- unrelated reporting/PDF behavior

This bug is specifically about the OPTIONS SOURCE for the FHN NAICS INDUSTRY dropdown.

==================================================
VALIDATION
==================================================

After implementing:

1. Confirm:

GET /api/v1/lookups/fhn-naics-industries

now obtains its values from:

[01_DATA_01_Data Mart Trial].[IntRepCM].[SubCategory]

2. Confirm returned values are DISTINCT.

3. Confirm NULL/blank handling follows existing project conventions.

4. Confirm the existing frontend still consumes the same endpoint with NO frontend modification.

5. Confirm no other lookup query or behavior changed.

6. Run relevant build/compile checks for modified code if safely available.

7. Show the exact git diff.

8. Report every modified file.

If more than ONE file needs modification:

STOP BEFORE modifying additional files and explain exactly why each additional file is technically necessary.

Do not proceed with unnecessary changes.

==================================================
FINAL REPORT
==================================================

Provide:

A. Root cause confirmed
B. Exact change made
C. Old query/source
D. New query/source
E. Files modified
F. Why each modified file was necessary
G. Validation performed
H. Regression impact assessment
I. Exact git diff summary

Do NOT commit or push.

Implement Bug #128 now using the smallest safe change.

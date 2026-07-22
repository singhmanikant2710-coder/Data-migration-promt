Task: READ-ONLY investigation for UAT Bug #128.

STRICT INSTRUCTIONS:
- READ ONLY.
- DO NOT edit, create, delete, rename, reformat, or modify any file.
- DO NOT modify anyone's work, including Jothi's changes.
- DO NOT implement a fix.
- DO NOT refactor or clean up code.
- DO NOT modify database objects, stored procedures, views, tables, APIs, or configuration.
- Do not run destructive commands.
- Findings only, with exact file paths and exact relevant code excerpts.

UAT BUG #128

Screen/Tab:
Review Form / Customer Info

Section/Field:
Customer Profile

Issue Description:
The "FHN NAICS INDUSTRY" dropdown values should be populated based on a DISTINCT SELECT of:

[01_DATA_01_Data Mart Trial].[IntRepCM].[SubCategory]

Currently, the dropdown is not connected to any selections / is not correctly populated from the required source.

GOAL OF THIS INVESTIGATION:
Trace the complete existing data flow for the "FHN NAICS INDUSTRY" dropdown from frontend -> API/service -> backend/repository -> database, and determine the smallest safe change required to fix Bug #128.

REPORT THE FOLLOWING:

1. FRONTEND LOCATION AND DROPDOWN IMPLEMENTATION

Find the Review Form / Customer Info screen and locate the exact implementation of the "FHN NAICS INDUSTRY" field/dropdown.

Report:
- Exact file path(s)
- Exact JSX/TSX for the label and dropdown/select control
- The state/form field bound to the selected value
- Where the dropdown options currently come from
- Whether options are:
  - hardcoded
  - empty
  - loaded from an API
  - derived from another field
  - incorrectly mapped

Paste the exact relevant code.

2. FRONTEND DATA LOADING

Trace how dropdown/reference/master data is loaded for this page.

Report:
- useEffect/hooks/functions involved
- API client/service calls
- endpoint URL/path used
- response property used for options
- TypeScript interfaces/types involved

Paste exact relevant code with file paths.

Determine specifically why "FHN NAICS INDUSTRY" is currently not populated/connected correctly.

3. BACKEND/API INVESTIGATION

Search for any existing API, controller, endpoint, service, handler, repository, or query that provides:
- NAICS Industry
- SubCategory
- IntRepCM
- Customer Profile dropdown/master/reference data

Report exact file paths and code.

Determine whether an existing endpoint/query can already provide the required values.

Do NOT assume a new endpoint is required.

4. DATABASE SOURCE

Required source is:

[01_DATA_01_Data Mart Trial].[IntRepCM].[SubCategory]

Find any existing code/query that accesses:
[01_DATA_01_Data Mart Trial].[IntRepCM]

Show the exact existing SQL/query if found.

Determine whether DISTINCT SubCategory values are already queried anywhere.

The expected logical dataset is equivalent to:

SELECT DISTINCT [SubCategory]
FROM [01_DATA_01_Data Mart Trial].[IntRepCM]
WHERE [SubCategory] IS NOT NULL

Do NOT implement this query.

Also report whether existing project conventions normally:
- exclude NULL values,
- exclude blank/empty strings,
- trim values,
- sort dropdown values.

Do not invent additional filtering if it does not already follow project conventions.

5. DEPENDENCIES / CASCADING DROPDOWNS

Check whether "FHN NAICS INDUSTRY" is supposed to depend on another Customer Profile selection, or whether any other dropdown depends on it.

Specifically identify:
- parent dropdown, if any
- child/dependent dropdown, if any
- existing onChange behavior
- filtering logic
- whether changing this dropdown affects any other fields

This is important to avoid breaking existing Customer Profile behavior.

6. SAVE/EDIT BEHAVIOR

Trace how the selected FHN NAICS INDUSTRY value is:
- loaded for an existing review
- stored in frontend state
- submitted/saved
- mapped in backend/DTO/database

Confirm whether Bug #128 concerns ONLY populating dropdown options or whether changing the option source would require changes to save/load behavior.

Do NOT change anything.

7. SIMILAR DROPDOWN PATTERN

Find one existing working dropdown on the same screen/application that loads DISTINCT/reference values from the database.

Show:
- frontend code
- API call
- backend/repository query

Use this only to identify the existing project pattern.

8. LIVE DB / SCHEMA VALIDATION

Use the project's actual/live configured database/schema information where safely available.

Ignore columns.csv or stale generated schema documentation.

Verify:
- exact database/schema/table reference for IntRepCM
- exact column name SubCategory
- data type if discoverable safely
- whether duplicate, NULL, or blank values exist if this can be checked read-only

READ-ONLY queries only.

9. ROOT CAUSE

State the exact root cause of Bug #128 based only on code evidence.

Clearly state whether the issue is:
- frontend binding/options issue,
- missing API call,
- backend query issue,
- incorrect DB source,
- or a combination.

10. MINIMAL FIX IMPACT ANALYSIS

Without changing anything, state:

- Exact file(s) that would need modification
- Expected number of files
- Whether frontend change is required
- Whether backend change is required
- Whether a new endpoint is required or an existing endpoint can be reused
- Whether SQL/repository change is required
- Whether any DTO/interface changes are required
- Any regression risk to other Customer Profile fields/dropdowns

IMPORTANT:
Do not implement the solution.

OUTPUT FORMAT:

A. Root Cause
B. Current Data Flow
C. Frontend Findings + exact code/file paths
D. Backend/API Findings + exact code/file paths
E. Database Findings
F. Save/Edit Impact
G. Dependencies
H. Existing Similar Pattern
I. Exact Minimal Change Required
J. Files That Would Need Modification
K. Risks / Regression Areas

End with:
"READ-ONLY INVESTIGATION COMPLETE — NO FILES MODIFIED"

Before finishing, run git diff/status only to confirm that this investigation itself made no changes. Do not modify or revert any pre-existing changes.

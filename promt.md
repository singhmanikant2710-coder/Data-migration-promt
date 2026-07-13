UAT #53 follow-up — filter Finding Codes to Active only.

Client: "We need to restrict Finding Code options to Active = Yes/True."

Confirmed on the LIVE DB: dbo.[03_LIBRARY_01_CAS Findings] has an [Active] column of type BIT with values 0 and 1 (65 rows are Active=1, 2 rows are Active=0). Ignore columns.csv.

Task:
In the backend findings library query (SqlFindingsRepository.cs — the one serving GET /api/v1/findings/library), add a filter so only rows with [Active] = 1 are returned.

Note the impact and confirm it is correct:
- This endpoint feeds BOTH the Finding Code dropdown labels (#53) AND the CAS CRM Findings Library modal (#56). Both will now show only active findings — that IS the intended behaviour.
- It should remove the legacy "CS-116OLD" entry we noticed earlier.

Important — do NOT break existing saved data: if a review already has a finding row whose code is now inactive (e.g. CS-116OLD), that saved value must still display in the dropdown for that row rather than appearing blank. Check whether the existing ensureIncludesSelected / label-fallback logic already handles this; if not, tell me — do not silently drop the saved value.

Show me the diff. STOP after applying.

Apply this edit now. Do not read other files. Do not plan. Just apply.

FILE: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewRepository.cs

In the method GetFindingCodesLookupAsync (the query that populates response.lookups.findingCodes, which feeds the Finding Code dropdown options), add a filter so only active findings are returned:

  WHERE [Active] = 1

Table: dbo.[03_LIBRARY_01_CAS Findings]. The [Active] column is BIT (65 rows are 1, 2 rows are 0 — including the legacy CS-116OLD which must disappear from the dropdown).

Use the LIVE DB. Ignore columns.csv — it is stale.

Apply this single edit and show me the diff. STOP.

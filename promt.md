Single-file edit only:
backend/src/Casrr.Infrastructure/Repositories/SelectionRepository.cs

Method: GetSelectionsByTabAndSectionAsync

GOAL: The "Report Name Selection" dropdown must return report names ordered 
by Selection_id ASC (per Geoff, issue #170), not alphabetically. Currently 
the query does ORDER BY [Selection] (alphabetical).

CONSTRAINT: The query uses SELECT DISTINCT [Selection], so ORDER BY 
[Selection_id] is not directly valid (DISTINCT requires ORDER BY columns to 
be in the SELECT list). Refactor to GROUP BY instead of DISTINCT, ordering 
by the minimum Selection_id per Selection.

Current SQL:
    SELECT DISTINCT LTRIM(RTRIM([Selection])) AS [Selection]
    FROM dbo.[03_LIBRARY_09_Selections] WITH (NOLOCK)
    WHERE LTRIM(RTRIM([Tab])) = LTRIM(RTRIM(@tab))
      AND LTRIM(RTRIM([Section])) = LTRIM(RTRIM(@section))
      AND [Selection] IS NOT NULL AND LTRIM(RTRIM([Selection])) <> ''
    ORDER BY [Selection];

Change to:
    SELECT LTRIM(RTRIM([Selection])) AS [Selection]
    FROM dbo.[03_LIBRARY_09_Selections] WITH (NOLOCK)
    WHERE LTRIM(RTRIM([Tab])) = LTRIM(RTRIM(@tab))
      AND LTRIM(RTRIM([Section])) = LTRIM(RTRIM(@section))
      AND [Selection] IS NOT NULL AND LTRIM(RTRIM([Selection])) <> ''
    GROUP BY LTRIM(RTRIM([Selection]))
    ORDER BY MIN([Selection_id]);

This preserves the distinct-by-Selection behavior (via GROUP BY), still 
returns only the trimmed Selection string, and orders by Selection_id ASC.

CONSTRAINTS:
- Edit ONLY this SQL string in this one method. Do NOT change the method 
  signature, return type (still IReadOnlyList<string>), parameter binding, 
  the row-reading logic (still reads a single [Selection] column), or 
  WITH (NOLOCK).
- Do NOT touch the controller, the frontend, the fallback array, or any 
  other file.
- Keep the same @tab/@section parameters and trimming logic.

After edit, show the final SQL and confirm the C# reader still maps the 
single [Selection] column.


SELECT [Selection_id], [Selection]
FROM dbo.[03_LIBRARY_09_Selections]
WHERE LTRIM(RTRIM([Tab])) = 'Reporting'
  AND LTRIM(RTRIM([Section])) = 'Report Selections'
ORDER BY [Selection_id];

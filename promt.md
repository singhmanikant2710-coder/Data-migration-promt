Single-file edit only:
backend/src/Casrr.Infrastructure/Repositories/SelectionRepository.cs

Method: GetSelectionsByTabAndSectionAsync (class SqlSelectionRepository)

GOAL (issue #170, Option A approved by Geoff): Order the returned selection 
values by Selection_id ascending instead of alphabetically by Selection.

CONSTRAINT: Current query uses SELECT DISTINCT [Selection], so 
ORDER BY [Selection_id] is invalid directly. Refactor DISTINCT to GROUP BY 
and order by MIN(Selection_id) per Selection.

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

CONSTRAINTS:
- Edit ONLY this SQL string in this one method. Do NOT change the method 
  signature, return type (IReadOnlyList<string>), parameter binding, the 
  single-[Selection]-column reader logic, or WITH (NOLOCK).
- Do NOT touch GetAllAsync, GetByIdAsync, the controller, the frontend, the 
  fallback array, or any other file.

After edit, show the final SQL and confirm the C# reader still maps only 
the [Selection] column.

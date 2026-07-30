READ-ONLY. Diagnostics only. Do not change anything.

I need to know the FULL blast radius of GetSelectionsByTabAndSectionAsync 
before changing its ORDER BY.

1. Search the entire codebase for every caller of:
   - the backend method GetSelectionsByTabAndSectionAsync
   - the endpoint GET /api/v1/selections/values
   - the frontend helper(s) that hit that endpoint (getReportNames and any 
     others in reporting.ts or elsewhere)

2. For EACH caller, report the exact (tab, section) pair passed. 
   For example: (Reporting, Report Selections), and any others like 
   (SomeTab, SomeSection).

3. List every UI dropdown / screen that is populated by this method, with 
   its file path.

Output a simple table: Caller file → tab value → section value → which 
dropdown it fills.

Do not edit any file. Findings only.

READ-ONLY DIAGNOSTIC. Do NOT edit, create, or delete any file. Do not run any commands. Only read and report.

CONTEXT: Bug — "When downloading a PDF for a summary of the rolling 24 months, it only populates 6 months." The rolling-24-months PDF summary shows only 6 months of data instead of 24. I need you to trace where the month count is limited to 6 (or where 24 is expected but truncated).

TASK: Trace the full data path for the Blackbook rolling-24-months PDF summary, from data source to PDF render, and report findings ONLY. Make ZERO changes.

Search and report on the following, quoting exact file paths + line numbers + the relevant code snippet for each:

1. BACKEND — the summary data source:
   - In backend/src/Bcat.Api/Controllers/BlackbookSummaryController.cs: find the endpoint that returns the rolling summary. Report its route, params, and what it calls.
   - In backend/src/Bcat.Application/Services/BlackbookSummaryService.cs and IBlackbookSummaryService.cs: find the method building the summary. Report how many months it fetches/returns.
   - In backend/src/Bcat.Application/Services/Calculations/TblMainCalcs.cs: check if any TTM / rolling / month-window logic caps the range.
   - Search the whole backend for any literal "6", "Take(6)", "TOP 6", "MonthsBack", "24", "rolling", "trailing" that could bound the month window. List every match with path:line.

2. BACKEND — the repository/query:
   - In backend/src/Bcat.Infrastructure/SqlServer/* and Access/* repositories, find the query that pulls monthly rows for the summary. Report any TOP/FETCH/LIMIT, date filter, or month-count parameter. Quote the SQL and the C# that sets the range.

3. FRONTEND — report + PDF render path:
   - In frontend/src/app/blackbook/report/page.tsx: find where summary data is fetched and passed to the PDF. Report any .slice(), .take, array length caps, or "6"/"24" literals.
   - Find the PDF summary component (likely a *PDF.tsx using @react-pdf/renderer that renders the rolling-24-month table). Report how it maps months to columns/rows and whether it caps the count.
   - Report any month-window constant / config used by the frontend summary.

OUTPUT FORMAT:
- A) Exact data path as a numbered chain: source query -> service -> controller -> API -> frontend fetch -> PDF component.
- B) The single most likely place the count is limited to 6, with file path + line number + quoted code.
- C) Any secondary suspects (ranked), each with path:line.
- D) Confirm whether the 24-vs-6 limit is on the backend (data never returned) or frontend (data returned but not rendered). State which, based on evidence.
- E) Do NOT propose or write a fix yet. Findings only.

READ-ONLY. Do not edit anything. Diagnostics only.

Context: The "Report Name Selection" dropdown on the Reporting screen 
(localhost:3100/reports) currently lists report names in what appears to be 
alphabetical order. Geoff wants them ordered by [Selection_id] ascending 
instead.

Report back — DO NOT change any code:

1. Locate the Reporting page component (likely frontend, e.g. 
   frontend/src/app/reports/page.tsx or a ReportSelection component). 
   Identify where the "Report Name Selection" dropdown options come from — 
   a static array, a backend API call, or a DB-backed list.

2. If backend-backed: find the repository method + SQL that returns the 
   report names. Show its SELECT, the columns returned (confirm whether a 
   Selection_id column exists), and its current ORDER BY (if any).

3. If frontend static/array or frontend-sorted: show the array definition 
   or any .sort() applied to the options before rendering, and confirm 
   whether each option carries a Selection_id field.

4. Report exactly:
   a. The source of the dropdown options (file + method/array).
   b. Whether a Selection_id field/column is available on each option.
   c. The current ordering logic (explicit ORDER BY, a JS .sort(), or 
      default/insertion order).
   d. The single place where changing the sort to Selection_id ASC would 
      take effect.

Do not edit any file. Output findings only.

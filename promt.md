File to modify: backend/src/Casrr.Infrastructure/SqlServer/SqlReviewHistoryRepository.cs
Method: GetHistoryRowsAsync

The Review History sample dropdown is empty and the "Sample / Review Name" 
column shows no value. Root cause: the query LEFT JOINs 02_CORE_01_Samples (s) 
to 02_CORE_02_Reviews (r) ON r.Sample_id = s.Sample_id, but these two tables 
use different Sample_id schemes so the join never matches and s.Sample_name 
comes back NULL. The Reviews table (02_CORE_02_Reviews) already has its own 
Sample_name column.

FIX (3 changes):
1. Remove the line: 
   LEFT JOIN dbo.[02_CORE_01_Samples] AS s WITH (NOLOCK) ON r.[Sample_id] = s.[Sample_id]
2. In the SELECT list, replace "s.[Sample_name] AS SampleName" with 
   "r.[Sample_name] AS SampleName".
3. In the WHERE clause, change "s.[Sample_name] LIKE '%' + @sampleName + '%'" 
   to "r.[Sample_name] LIKE '%' + @sampleName + '%'".

Keep everything else identical (all r.* columns, finalized filter, cancelled 
filter, borrowerName filter, ORDER BY). Modify ONLY this file. Do not touch any 
other file. After editing, paste back the full changed query.



UI Changes -----

File to modify: frontend/src/app/review-history/page.tsx

Apply these 4 UI changes (these were done before but got reverted). Reference 
the review-status page for the icon position and pagination patterns.

CHANGE 1 — Remove the radio/circle in each row:
The circle is NOT in this file — it comes from the shared DataTable component 
(@/components/table/DataTable), rendered as a default selection column. Check 
whether DataTable accepts a prop to disable it (e.g. selectable={false} or 
showSelection={false}). If such a prop exists, pass it from this page's 
<DataTable .../> usage to hide the circle. If NO such prop exists, STOP and 
tell me — do NOT edit DataTable.tsx without asking.

CHANGE 2 — eCIF# column: remove the "-" placeholder. Show the value when 
present; render an empty cell when null/empty (no dash).

CHANGE 3 — Move the document/pdf icon to BEFORE the borrower name (to the left 
of the name), matching how the review-status page places the icon before the 
borrower name. Look at review-status/page.tsx's borrower cell for the exact 
layout/structure and mirror it here.

CHANGE 4 — Replace the pagination with the same style used on the review-status 
page (Previous / page numbers / Next, with Rows per page). Look at how 
review-status/page.tsx renders its pagination and apply the same component/
pattern here so both screens are consistent.

IMPORTANT — keep the long-borrower-name truncation working: the borrower name 
cell must cap its width and truncate long names so they don't overlap the icon. 
The DataTable <td> has whitespace-nowrap, so the borrower name cell's 
cellClassName needs "!whitespace-normal max-w-[280px] w-[280px] overflow-hidden" 
and the name should truncate.

CONSTRAINTS:
- Modify ONLY frontend/src/app/review-history/page.tsx (except CHANGE 1, which 
  may need a prop — if it requires editing DataTable.tsx, STOP and ask).
- Do NOT change backend, API calls, or data logic.
- Keep dropdown filter, borrower search, and sorting working.

After editing, summarize what changed in each of the 4 areas.

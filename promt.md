UAT #54 — CRM Findings: show the Finding Category in the INFO column.

IMPORTANT — the client has CHANGED this requirement. The original ask (an Info hyperlink showing Code/Description/Guidance) is NO LONGER needed. The new requirement is:

"Can we show the Finding_category where the Info column is currently? The maximum value for this field in the Findings Library is 35 characters long. This column can be wide enough to display the full Finding_category over two rows when wrapped."

So: keep the column header as "INFO" (do NOT rename it). Under that header, each row should display the FINDING CATEGORY text for that row's selected finding code — wrapped over up to two lines. No popup, no hyperlink.

CONTEXT (reuse — do not refetch):
- The CAS Findings library is already fetched client-side in CrmFindingsAndRatingsSection.tsx (from the UAT #53 work). Each library item contains: component, findingCode, description, CATEGORY, guidance.
- Currently only labelMap ("CODE - Description") is retained. Check whether `category` is still reachable in this component; if not, build a categoryMap (component -> code -> category) from the SAME existing fetch. No new API call.

YOUR TASK:
1. FIRST report (read-only, no edits):
   a. What the INFO column cells currently render (exact JSX).
   b. Whether `category` from the library fetch is still available, or the minimal change to keep it.
2. THEN propose ONE minimal fix:
   - Header stays "INFO".
   - Each row's INFO cell shows the category for that row's selected finding code.
   - Column wide enough to fit ~35 characters across up to TWO wrapped lines (whitespace-normal, break-words, no truncation).
   - Blank cell if no finding code is selected yet.

Requirements:
- No new API calls, no fetch loops.
- DISPLAY ONLY — no change to save logic or persisted data.
- Works for all components (CS-*, SS-*, etc.).
- Styling consistent with the existing table (dark navy #1F3864 header, same padding/font).
- The table must NOT gain horizontal scroll.

Report findings and plan with exact files touched. STOP and wait for approval.

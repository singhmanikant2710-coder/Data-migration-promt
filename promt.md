READ-ONLY. Diagnostics only. Do NOT change anything.

File: frontend/src/app/load-samples/page.tsx (freshly reverted to clean state).

Show me the ACTUAL current code (literal JSX + line numbers, NOT comments/summaries):

1. The Select Sample grid: the <div> wrapper containing the <DataTable> with columns={parentColumns} — show from that wrapper div through DataTable + Pagination to closing </div>. Note: overflow-x-auto? min-w? DataTable className?

2. The parentColumns definition: show the Type column (the <Select> with options), the EIC Name and Target (BU) columns (SearchableSelect), and the Quarter + Closed columns. Show their current width classNames and whether there's any isCreating flag.

3. Confirm: is the Type dropdown's first <option> empty (value="" with no label) or does it say "Select..."?

4. The NEW_ID constant value and how editingParentId compares to it.

Paste real code with line numbers. No comments like /* Grid */. Read only.

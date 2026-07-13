UAT #39 — Review Queue: the "Filter rows" search must also update the "Progress Status" grid totals.

Client requirement:
"Can the 'Google' search Filter rows feature update the 'Progress Status' grid totals?"

(Note: the client also asked "What does the Clear button do? It does not seem to clear out comments" — that appears to be a copy-paste from another item, since there are no comments on Review Queue. Ignore it for now unless you find a Clear button on this screen that is genuinely broken — if you do, report what it currently does.)

YOUR TASK:
1. FIRST report (read-only, no edits):
   a. Find the Review Queue screen component. Show me the "Filter rows" search input and how it filters the table rows (client-side filter on the loaded rows, or a server-side query?).
   b. Show me the "Progress Status" grid/totals — where do those numbers come from? Are they derived from the same row array the table renders, or computed separately (e.g. from a different API call or from the unfiltered dataset)?
   c. Confirm why the totals do not currently react to the filter.

2. THEN propose ONE minimal fix so the Progress Status totals are derived from the FILTERED rows, updating live as the user types in the search box (and resetting when the search is cleared).

Constraints:
- Prefer a client-side derivation from the already-filtered row array — no new API calls if the data is already loaded.
- Display-only: no changes to any save path or persisted data.
- The totals must return to the full counts when the filter is cleared.

Report findings and plan with exact files touched. STOP and wait for approval.

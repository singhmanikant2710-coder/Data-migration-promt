Approved with one correction — apply the plan, but with this change:

The progress counts must be derived from the rows AFTER the "My View" scope and the text search (filterQuery) are applied, but BEFORE statusFilter is applied.

Reason: if a user clicks a status in the Progress Status grid (setting statusFilter), deriving the counts from the fully-filtered rows would collapse the grid to show only that status with a count, and 0 for everything else. That would be confusing and would break the grid's purpose as a navigation aid.

So: create an intermediate memo (e.g. scopedSearchedRows) = rows after My View scope + filterQuery text search, WITHOUT statusFilter. Derive progressCountsFromScoped and totalsUI from that array. Keep filteredRows (which additionally applies statusFilter) for the TABLE rows only.

Everything else as planned:
- Overlay derived counts onto the server-provided progressCounts to preserve ordering/highlight, appending any statuses not in the server list.
- Render the panel from progressCountsUI and totalsUI.
- Display-only, no new API calls, no save-path changes.
- Clearing the search restores the full counts for the current scope.

Apply and show me the diff. STOP after applying.

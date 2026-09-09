Apply all the diffs exactly as shown:
- Fix 1: lookups.ts (both functions — forceRefresh param, delete cache + inflight, skipCache: forceRefresh)
- Fix 2a: blackbook/edit/page.tsx (import + handleRefreshSave force-refresh + handleAddNewMonth cache purge)
- Fix 2b: customer/edit/page.tsx (init effect both calls true)
Leave covenants/edit and all other call sites unchanged (forceRefresh defaults false).
Apply now, then run the frontend typecheck/build and report any errors.

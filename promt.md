Option B — show me the final diffs first, apply nothing yet.

Prepare (but do NOT apply) the diffs for:
- Fix 1: lookups.ts (getMaxMonthKey + getMaxMonthKeyWithCovenants — forceRefresh param, delete cache AND inflight, skipCache: forceRefresh)
- Fix 2a: blackbook/edit/page.tsx (import getMaxMonthKeyWithCovenants; force-refresh in handleRefreshSave and handleAddNewMonth)
- Fix 2b: customer/edit/page.tsx (init effect passes true to both)

Leave covenants/edit UNCHANGED (per your analysis it's unnecessary).

Show me all the unified diffs. I'll review and confirm before you apply.

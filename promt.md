Apply all of the following. Show final diffs, then apply.

FIX 1 (lookups.ts): Add forceRefresh param to getMaxMonthKey and getMaxMonthKeyWithCovenants. When forceRefresh:
- delete BOTH window.__bcat_cache[key] AND window.__bcat_inflight[key] (not just cache — a stale in-flight promise must be dropped too).
- pass skipCache: forceRefresh into the inner get() call (to bypass the 1500ms api.ts TTL cache).
Apply exactly as in the proposed diff (both functions).

FIX 2a (blackbook/edit/page.tsx):
- Add getMaxMonthKeyWithCovenants to the import.
- In handleRefreshSave, AFTER the monthkey-series refresh, add:
    try {
      const mkFresh = await getMaxMonthKey(name.trim(), true);
      await getMaxMonthKeyWithCovenants(name.trim(), true);
      if (mkFresh) setMaxMonthKey(mkFresh);
    } catch {}
- In handleAddNewMonth, keep setMaxMonthKey(mk) and ADD after it:
    try { await getMaxMonthKey(name.trim(), true); await getMaxMonthKeyWithCovenants(name.trim(), true); } catch {}
  (so the shared window cache is corrected, not just local state)

FIX 2b (customer/edit/page.tsx): In the monthKey init effect (L613-619), change:
    getMaxMonthKeyWithCovenants(qpName) -> getMaxMonthKeyWithCovenants(qpName, true)
    getMaxMonthKey(qpName) -> getMaxMonthKey(qpName, true)
  (this effect re-runs after profile reload/save, serving stale values).

ALSO (agent flagged this — do it too): covenants/edit/page.tsx L151 initMonthRange uses getMaxMonthKey(customerName) and that page saves covenants without busting the cache — this is a likely source of stale max-with-covenants seen later. Add a forced refresh there too if it's safe (quote it first; if it's a read-only init that runs once per mount with a fresh window, leave it — but if it runs after a covenant save on the same page, force-refresh).

DO NOT touch the pure read-only view/report call sites (blackbook/view, blackbook/report) — those don't need forceRefresh.

STRICT: forceRefresh defaults to false, so all existing callers are unaffected. Only the add/save/init paths pass true.

VERIFY BEFORE SHOWING DIFFS:
a) Both lookups delete cache AND inflight, and pass skipCache: forceRefresh.
b) handleRefreshSave and handleAddNewMonth now force-refresh the shared cache.
c) customer/edit init effect passes true.
d) forceRefresh defaults false — read-only callers unchanged.
e) Quote covenants/edit L151 context to decide if it needs true.

Show all diffs. Apply nothing until I confirm.

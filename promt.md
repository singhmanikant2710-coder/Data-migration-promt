Fix the maxMonthKey cache staleness. Show unified diffs BEFORE applying.

PROBLEM: getMaxMonthKey and getMaxMonthKeyWithCovenants (frontend/src/lib/lookups.ts) use getOnce (window.__bcat_cache). After Refresh/Save or Add, the cached value is stale, so the month/structure becomes inconsistent ("works, then wrong after refresh/save").

=== FIX 1: Add forceRefresh to both lookups (frontend/src/lib/lookups.ts) ===
For getMaxMonthKey, add an optional forceRefresh param that deletes the cache key before fetching:

    export async function getMaxMonthKey(customer: string, forceRefresh = false): Promise<string | null> {
        const cust = String(customer || "").trim();
        const key = `context:maxMonthKey:v3:${cust}`;
        if (forceRefresh && typeof window !== "undefined") {
            try { delete (window as any).__bcat_cache?.[key]; } catch {}
        }
        return await getOnce<string | null>(key, async () => {
            // ... existing fetcher body UNCHANGED ...
        });
    }

Do the SAME for getMaxMonthKeyWithCovenants (add forceRefresh param, compute its key, delete before getOnce). Keep the fetcher bodies unchanged.

=== FIX 2: Pass forceRefresh=true in the Refresh/Save and Add paths ===
Find every call to getMaxMonthKey / getMaxMonthKeyWithCovenants in:
- frontend/src/app/blackbook/edit/page.tsx (Refresh/Save handler handleRefreshSave, and handleAddNewMonth)
- frontend/src/app/customer/edit/page.tsx (monthKey init effect)

In the Refresh/Save handler and after Add, change getMaxMonthKey(name) → getMaxMonthKey(name, true) and getMaxMonthKeyWithCovenants(name) → getMaxMonthKeyWithCovenants(name, true), so they bust the cache and fetch fresh.

Do NOT change the initial context-load call's behaviour unless needed (that runs on customer change with a fresh window anyway) — but the Refresh/Save path MUST force-refresh.

Show me:
1) Updated getMaxMonthKey + getMaxMonthKeyWithCovenants with forceRefresh (Fix 1), quoted.
2) Every call site of these two functions, quoted, so I can confirm which get forceRefresh=true (Refresh/Save + Add paths).

Show diffs. Apply nothing until I confirm.

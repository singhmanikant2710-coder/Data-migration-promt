The diff preview is broken — it contains DUPLICATE `const arr` and `let def` declarations and mismatched braces from mixing old and new versions. That would cause "cannot redeclare arr/def" build errors. Do NOT apply that.

Instead, do a clean targeted replacement. In frontend/src/app/blackbook/edit/page.tsx, in the "Load month key series for selected year" effect, find this EXACT existing block:

        let def = selectedMonthKey;
        const mk = (maxMonthKey || "").trim();
        if (!def) {
            const mkp = monthKeyParam.trim();
            if (/^\d{6}$/.test(mkp) && arr.includes(mkp)) {
                def = mkp;
            } else {
                def = arr.includes(mk) ? mk : arr[arr.length - 1] || "";
            }
        }

Replace ONLY that block (do NOT touch the `const arr = ...` line above it, do NOT touch the `if (!cancelled) { setMonthKeys... }` below it) with:

        let def = selectedMonthKey;
        // If the user's current selection is valid within the loaded months, keep it (don't overwrite).
        if (def && arr.includes(def)) {
            // keep def as-is
        } else if (!def) {
            const mkp = monthKeyParam.trim();
            if (/^\d{6}$/.test(mkp) && arr.includes(mkp)) {
                def = mkp;
            } else {
                // default to the fiscal-latest month (arr is ASC), not maxMonthKey
                def = arr[arr.length - 1] || "";
            }
        } else {
            // def set but not in the loaded arr (year changed) -> fall back to last of arr
            def = arr[arr.length - 1] || "";
        }

CRITICAL:
- Do NOT add a second `const arr` — the existing `const arr = (Array.isArray(keys)...)` line stays exactly once, untouched.
- Do NOT add a second `let def` — only ONE `let def = selectedMonthKey;`.
- The `const mk = (maxMonthKey...)` line is REMOVED (no longer used) — confirm maxMonthKey isn't referenced elsewhere in this block after removal; if it causes an unused-var lint, that's fine, but do not leave a duplicate.
- Keep the `if (!cancelled) { setMonthKeys(arr); setSelectedMonthKey(def); setMkAttempted(true); setLastRefreshAt(new Date()); }` block exactly as-is below.

After applying, show me the COMPLETE effect from its `async function loadMonthKeys()` (or the `const arr =` line) through `setSelectedMonthKey(def);` — so I can verify exactly ONE `const arr`, exactly ONE `let def`, no duplicate declarations, valid braces.

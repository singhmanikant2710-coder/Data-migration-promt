Two bounded fixes in frontend/src/app/blackbook/edit/page.tsx. Show a unified diff for each BEFORE applying, then apply. Do not run build.

PROBLEM: BHG dropdown correctly lists 202510-202605, but selecting a 2026 month shows 202512's data, and "Month Key: 202512" is displayed. Cause: the default-month effect resets selectedMonthKey to maxMonthKey (which is 202512, a calendar-year max), overwriting the user's dropdown choice. The current-year series already contains 202601-202605 (backend is fixed), so we just need the frontend to (a) not overwrite a user selection, and (b) default correctly to the fiscal-latest month.

=== FIX 1: Don't overwrite the user's month selection ===
Find the month-key loading effect where the default month is computed and setSelectedMonthKey(def) is called. Currently it computes:
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
    ...
    setSelectedMonthKey(def);

The bug: even when selectedMonthKey is already set (user picked 202605), something resets it. Change the logic so that if selectedMonthKey is already a valid member of the newly loaded arr, we KEEP it and do NOT overwrite:
    let def = selectedMonthKey;
    const mk = (maxMonthKey || "").trim();
    // If the user's current selection is valid within the loaded months, keep it.
    if (def && arr.includes(def)) {
        // keep def as-is (do not overwrite the user's selection)
    } else if (!def) {
        const mkp = monthKeyParam.trim();
        if (/^\d{6}$/.test(mkp) && arr.includes(mkp)) {
            def = mkp;
        } else {
            // default to the LAST month in the fiscal-year series (arr is ASC), not maxMonthKey
            def = arr[arr.length - 1] || "";
        }
    } else {
        // def was set but not in arr (year changed) -> fall back to last of arr
        def = arr.includes(def) ? def : (arr[arr.length - 1] || "");
    }
    ...
    setSelectedMonthKey(def);

Key change: (1) if selectedMonthKey is already in arr, keep it; (2) default to arr[arr.length-1] (fiscal-latest) instead of maxMonthKey, so calendar-max 202512 no longer wins over 202605.

=== FIX 2: latestPoint must resolve strictly by selectedMonthKey ===
Find latestPoint useMemo:
    const match = mk ? src.find((p) => (p?.monthKey || "").trim() === mk) : null;
    return match || src[src.length - 1] || null;

This is fine IF selectedMonthKey is correct. With Fix 1, selectedMonthKey will be the user's month. Leave the find logic, but confirm src is the full series (containing 202601-202605), not a truncated enrichedSeries. If enrichedSeries drops months, prefer series. Quote enrichedSeries; if it caps/filters months, change latestPoint's src to always use the fuller of the two:
    const src = (series && series.length) ? series : (enrichedSeries || []);
(only if enrichedSeries is shorter/truncated — otherwise leave as-is and just report).

VERIFY BEFORE SHOWING EACH DIFF:
a) Fix 1: if selectedMonthKey is already in arr, it is NOT overwritten; default uses arr[arr.length-1], not maxMonthKey.
b) Fix 2: confirm whether enrichedSeries truncates months; only change src if it does. Report enrichedSeries contents.

Show both unified diffs. Apply nothing until I confirm.

SINGLE-FILE, BOUNDED EDIT. frontend/src/app/blackbook/edit/page.tsx, only the loadMonthKeys effect (~L726). Show diff BEFORE applying.

BUG (FY switch glitch): On FY change, selectedMonthKey isn't reset. loadMonthKeys seeds def from the OLD selectedMonthKey (e.g. 202603 from 2026), which is truthy, so the "pick new year's latest" branch is skipped, and it re-commits the stale month against the new year's list.

FIX (agent's tighter option): Validate selectedMonthKey against the newly loaded arr. If it doesn't belong to the new year, fall through to the existing "prefer maxMonthKey, else last" logic.

Find:
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

Change the first line so a selectedMonthKey NOT in the new arr is treated as empty:
    let def = arr.includes(selectedMonthKey) ? selectedMonthKey : "";
    const mk = (maxMonthKey || "").trim();
    if (!def) {
        const mkp = monthKeyParam.trim();
        if (/^\d{6}$/.test(mkp) && arr.includes(mkp)) {
            def = mkp;
        } else {
            def = arr.includes(mk) ? mk : arr[arr.length - 1] || "";
        }
    }

Only that one line change (def seed). Everything else unchanged.

VERIFY:
a) def = arr.includes(selectedMonthKey) ? selectedMonthKey : ""
b) If selectedMonthKey is in the new year's arr, it's kept (no unnecessary reset); if not, falls through to latest.
c) Nothing else changed.

Show diff. Apply nothing until I confirm.

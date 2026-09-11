Read-only this turn — diff only, no modifications.

Read-only. No files modified.

## First — a defect in what I applied last turn that you were right to question

**Adding `latestPoint` to the deps cannot overwrite manual state.** The `if (!next[lbl])` guard makes every re-run a no-op for any label that already holds a value, manual or otherwise. So on that axis it's safe.

**But the seed as currently applied is a no-op in practice.** Sequence on page load:
1. `isConsumerFinance` is true immediately (from the `industry` query param), while `series` is still empty and `latestPoint` is `null`.
2. Effect runs: `lv = {}` → all three stored selectors read as `""` → `norm("")` returns `def` → **all six labels get seeded to the Principal default**.
3. Row arrives, `latestPoint` changes, effect re-runs — and `if (!next[lbl])` is now false for all six, so the real stored basis is **never applied**.

So the fix I applied last turn doesn't actually do anything. The `latestPoint` dependency is necessary but not sufficient; the effect must also decline to seed until the row exists. Hunk 1 below fixes that. Keeping `latestPoint` in the deps is correct — removing it would make the stored basis permanently unreachable.

---

## Unified diff — 3 hunks, all in `frontend/src/app/blackbook/edit/page.tsx`

**Hunk 1 — seeding effect: don't seed from an empty row (L1910)**

```diff
@@ -1907,6 +1907,15 @@ (async () => {
     const opts = await getPrincipalOrGrossOptions();
     const options = (Array.isArray(opts) && opts.length > 0) ? opts : ["Principal N/R"];
     const def = options.find(o => /principal/i.test(o)) || options[0];
     if (cancelled) return;
+
+    // Do not seed before the row is available: seeding from an empty row would lock in
+    // "def" via the "if (!next[lbl])" guard below and permanently mask the customer's
+    // stored basis when it arrives. Options are still published so the dropdown renders.
+    if (!latestPoint) {
+      setPrincipalGrossOptions(options);
+      return;
+    }
+
+    // Seed each metric's basis from the customer's stored selector on the latest row.

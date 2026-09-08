The plan looks good — additive, handleSave untouched, sections untouched. Confirm before applying:

1. The clear() change in 3.4 — confirm it's ONLY in FormChangesContext (the draft layer), and does NOT modify handleSave or change when/how the existing save clears staged changes on success. The existing dirty-clear-on-successful-save behavior must be byte-identical.

2. Confirm the document-level capture guard does NOT interfere with: (a) the review's internal section tabs (buttons, not links), (b) any other screen's navigation, (c) the existing router.replace section navigation. It must only intercept Home / Review Queue / left-nav links WHEN dirty, and be a complete no-op when clean.

3. Confirm TopChromeBar changes are only 2 OPTIONAL props + a status badge — existing TopChromeBar usage on other screens (if any) stays working with the props absent.

4. Keep the 24h TTL (good balance for banking data on shared machines).

If all confirmed, apply — but show me the final diffs grouped by file. Do NOT auto-approve; I'll review each file's diff before it's committed. Run npm run lint + npm run build (no test deps — use node --test for the logic module only).

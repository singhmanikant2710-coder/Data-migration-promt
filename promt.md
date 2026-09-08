Implement Option (c) — exclude the auto-derived transaction enrichment values from the DIRTY/DRAFT computation ONLY, without changing the save payload or touching TransactionsSection/useTransactions. Contained entirely in frontend/src/lib/reviewDraft.ts (new module, no other consumers).

The false-dirty comes from load-time enrichment staging: transactions.<acctId>.collateralDesc, .businessTypeDesc, .purposeDesc, and _update:true — auto-derived, not user edits.

REQUIREMENTS:
1. Add WILDCARD support to the ignore logic (current REVIEW_DRAFT_IGNORED_PATHS only handles static paths). Support patterns like:
   - transactions.*.collateralDesc
   - transactions.*.businessTypeDesc
   - transactions.*.purposeDesc
   where * matches any account id.
2. Also treat _update as a non-meaningful marker in this context (a bucket containing only derived keys + _update is NOT a genuine edit).
3. CRITICAL rule for hasDraftableChanges / sanitizeDraftChanges: for each transaction row bucket:
   - If the bucket contains ONLY ignored/derived keys (collateralDesc, businessTypeDesc, purposeDesc, _update) → DROP the bucket (not dirty).
   - If the bucket ALSO contains a genuine user-edited key (anything else) → KEEP the bucket intact (dirty + restorable), including its derived keys.
   This ensures: load-time enrichment → clean; real transaction edit → still dirty and recoverable.
4. Do NOT change the save payload. Do NOT modify TransactionsSection.tsx, useTransactions.ts, or updateRow. The staging into `changes` stays exactly as-is (so collateralDesc etc. still persist on save). Only the DIRTY/DRAFT view of `changes` filters these out.
5. Keep it in reviewDraft.ts only. Add 3-4 node --test cases covering: bucket with only derived keys → not dirty; bucket with a real edit + derived keys → dirty and fully preserved; wildcard matches any acctId; the existing static ignore (repayment.analysis.activeDiscussionTab) still works.

Show the diff (reviewDraft.ts + new tests). Rebuild + run node --test. Do NOT commit. I'll re-test TEST 3a/3b/5 and a genuine transaction edit.

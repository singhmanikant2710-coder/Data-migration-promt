Confirmed: I saw the row come back AFTER Save (delete → Save → row visible → Ctrl+R → gone). Matches your root cause.

Approved, with one change to the approach: in usePolicyExceptions.ts:156-163, keep only rows that are genuinely unsaved local additions – identified the same way handleDeleteConfirm does (isNewPolicyExceptionId or staged as new) – instead of using the previous row count. If new rows cannot be identified that way, use your count rule and explain why.
- Rows added with Add and not yet saved must still be kept across reloads.
- Do not change anything else (#235 rules, #254 review-level notes, #232 changes untouched).
- Also check: staged edits are keyed by row id = position. After a delete + reload, can a staged edit end up on the wrong row? Report only, do not fix.

Report in chat: ADDED/CHANGED (file:line), before/after for: delete last row → Save; delete a middle row → Save; delete + add a new unsaved row → Save; add only → Save. npx tsc --noEmit, npm run build, git status. Do not commit or push.

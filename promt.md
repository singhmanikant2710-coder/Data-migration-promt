APPLY THE REVIEWED DIFF NOW — STRICTLY BOUNDED, NO EXTRA CHANGES.

We have confirmed the requirement and reviewed the current diff.

GOAL:
Fix ONLY the Consumer Finance Summary Top Strip display so that its percentage value follows the same Principal/Gross basis selected in the corresponding Cash Collections % / 60+ DPD % dropdown, matching the legacy application's behavior.

STRICT SCOPE:
- Only modify: frontend/src/app/blackbook/edit/page.tsx
- Apply ONLY the already-reviewed Top Strip diff (Hunk 1 + Hunk 2 shown in the previous diff).
- Do NOT create any new rows.
- Do NOT remove any existing rows.
- Do NOT change the layout or row structure.
- Do NOT change any backend file.
- Do NOT change MetricPoint or any TypeScript type.
- Do NOT change tblMain/API/DTO/mapping.
- Do NOT change calculation formulas.
- Do NOT change tblMainCalcs.
- Do NOT change the recompute loops.
- Do NOT change Add New Month logic.
- Do NOT change live-edit logic.
- Do NOT change month switching.
- Do NOT change edits/setEdits/commitIfChanged/chooseWriteKey.
- Do NOT add any clobber/guard logic outside the reviewed diff.
- Do NOT modify View/Report behavior.
- Do NOT modify non-Consumer-Finance behavior.

IMPORTANT EXISTING-BEHAVIOR SAFETY:
The existing calculation path MUST remain untouched.

For Add New Month and live editing:
- When the user enters/changes a value, the existing pendingVal must continue to have priority.
- Existing frontend formulas/recompute behavior must continue to update cells instantly exactly as before.
- A user-entered/manual value must NEVER be replaced by the new display override.
- The new override is DISPLAY ONLY for the Consumer Finance percentage Top Strip tiles.
- The existing middle-panel Cash Collections / Net C/O / 60+ DPD calculations and dropdown behavior must remain unchanged.

IMPLEMENTATION REQUIREMENT:
Use the reviewed logic exactly:

1. Keep:
   const pendingVal = (editMode && mk && writeKey) ? edits[mk]?.[writeKey] : undefined;

2. Keep:
   const effectiveVal = pendingVal !== undefined ? pendingVal : t.value;

3. For Consumer Finance percentage Top Strip tiles only:
   - call computeConsumerFinancePercentOverride(...)
   - pass latestPointComputed?.values || {}
   - pass principalGrossByLabel
   - pass principalGrossOptions
   - only use the returned override when it is not null.
   - only run this override when pendingVal === undefined.
   - therefore a pending user edit ALWAYS wins.

4. Keep the existing display formatting behavior for:
   - YTD PBT
   - Inventory Turn
   - A/R Turn Days
   - all other non-CF percentage/currency/ratio tiles.

5. Apply the same effectiveVal/display expression to the existing read-only <span> so that the Summary Top Strip displays the same basis-aware value.

6. Do NOT alter the edit <input> behavior or its default value logic beyond what is already included in the reviewed diff.

SEED FIX:
The previously reviewed stored-selector seed fix is already present and must NOT be removed or rewritten.
It must continue to:
- wait until latestPoint exists,
- read the customer's stored Cash Collections / 60+ DPD / Net C/O selectors,
- seed both $ and % labels,
- preserve the existing `if (!next[lbl])` guard,
- never overwrite a user's manual dropdown selection.

DO NOT ADD ANY OTHER FIX.

BEFORE APPLYING:
- Inspect the current file and make sure the reviewed diff still matches the current code.
- If the current code has materially changed and the reviewed diff cannot be applied safely, STOP and show me the conflict/difference instead of guessing.

AFTER APPLYING:
1. Show the exact `git diff -- frontend/src/app/blackbook/edit/page.tsx`.
2. Confirm that ONLY the intended Top Strip changes were applied.
3. Run `git diff --check`.
4. Confirm there are no changes to any other file.
5. Do NOT refactor, clean up, optimize, or make any additional changes.
6. Do NOT modify anything if the diff contains unexpected changes.
7. Do NOT run a build yet. I will explicitly confirm before the build/test step.

FINAL VERIFICATION:
Confirm explicitly:
- No rows added.
- No rows removed.
- No calculation logic changed.
- No Add New Month logic changed.
- No live-edit/recompute logic changed.
- Manual/pending values still have priority.
- Principal/Gross dropdown basis now controls the corresponding Consumer Finance Summary percentage display.
- Non-Consumer-Finance behavior remains unchanged.
- Only frontend/src/app/blackbook/edit/page.tsx was modified.

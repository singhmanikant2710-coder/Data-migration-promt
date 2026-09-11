STRICT REGRESSION-SAFE REQUIREMENT:

This fix MUST NOT change any existing formula/recompute behavior.

The application currently has an important live-calculation behavior:
when the user enters/changes a value while adding a new month, frontend formulas immediately recompute dependent cells and the UI updates instantly.

DO NOT modify, refactor, move, or bypass any of that logic.

DO NOT modify:
- formula calculation functions
- recompute loop
- pending edit handling
- edits state
- month creation logic
- month switching logic
- t.value generation
- aliases
- backend/API
- MetricPoint type
- existing calculation formulas
- existing middle-panel calculation behavior

The fix is ONLY to synchronize the Summary Top Strip display with the already-existing calculated value.

SAFE DISPLAY RULE:

For Top Strip:
    let effectiveVal = pendingVal !== undefined ? pendingVal : t.value;

For Consumer Finance percentage tiles ONLY, when there is no pending value for that tile, use the existing
computeConsumerFinancePercentOverride(...) result.

Conceptually:

    let effectiveVal = pendingVal !== undefined ? pendingVal : t.value;

    if (
      pendingVal === undefined &&
      isConsumerFinance &&
      String(t.kind || "").toLowerCase() === "percent"
    ) {
      const ov = computeConsumerFinancePercentOverride(
        String(t.label || ""),
        (latestPointComputed?.values || {}) as any,
        principalGrossByLabel,
        principalGrossOptions
      );

      if (ov !== null) {
        effectiveVal = ov;
      }
    }

IMPORTANT:
- pendingVal MUST always win over the override.
- This protects existing Add New Month/live-edit behavior.
- The override is only for the Summary Top Strip display when there is no pending value.
- Do not change the calculation itself.

Then replace ONLY the Top Strip's inline display expression:

    pendingVal !== undefined ? pendingVal : t.value

with:

    effectiveVal

at the previously identified six display locations.

Keep the existing formatting exactly as-is:
- YTD PBT -> formatCurrencyExact(effectiveVal)
- Inventory Turn / A/R Turn Days -> formatNoDecimals(effectiveVal)
- all other tiles -> renderTopStripValue(t.kind, effectiveVal)

EXPECTED RESULT:

1. Existing Add New Month:
   User enters a value -> formulas recalculate instantly -> existing UI behavior remains exactly unchanged.

2. Cash Collections:
   Principal N/R -> 5.82%
   Gross N/R -> 4.50%
   Middle panel and Summary Top Strip show the same value.

3. 60+ DPD:
   Principal N/R -> 4.88%
   Gross N/R -> 3.51%
   Middle panel and Summary Top Strip show the same value.

4. Net C/O:
   Principal N/R -> 0.58%
   Gross N/R -> 0.44%
   Middle panel and Summary Top Strip show the same value.

5. Changing one dropdown must NOT change the other metrics.

6. User-entered/pending values must never be replaced by the Consumer Finance override.

7. No existing live formula/recompute behavior may change.

BEFORE APPLYING:
- Show unified diff only.
- Do NOT modify files.
- Do NOT run build.
- Do NOT run tests.
- Wait for explicit confirmation.

Also inspect the existing seeding effect carefully before changing its dependency array. Do NOT add latestPoint as a dependency if doing so can overwrite or reinitialize the user's current/manual selector state when month data changes. The stored selector should be used only to initialize a missing selector state for the relevant row/month.

The final change must remain bounded to frontend/src/app/blackbook/edit/page.tsx.

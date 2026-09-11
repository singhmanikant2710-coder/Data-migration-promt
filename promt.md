We need to fix ONE specific UI synchronization bug in:

frontend/src/app/blackbook/edit/page.tsx

DO NOT change backend, API, DTO, formulas, recompute logic, Add New Month logic, or any existing calculation behavior.

CURRENT BEHAVIOR:
In the new application, the middle "Cash & Charge-offs" panel works correctly.

Example:
- Cash Collections % = Principal N/R -> 5.82%
- Change dropdown to Gross N/R -> middle panel immediately becomes 4.50%

BUT the Summary Top Strip still shows the old 5.82%.

Same problem exists for 60+ DPD:
- Principal N/R -> 4.88%
- Gross N/R -> 3.51%
The middle panel changes, but Summary Top Strip does not follow it.

LEGACY BEHAVIOR:
The Summary Top Strip follows the selected dropdown immediately:
- Cash Collections: Principal 5.82% / Gross 4.50%
- 60+ DPD: Principal 4.88% / Gross 3.51%

IMPORTANT REGRESSION REQUIREMENT:
The application has existing live formula behavior.

When the user enters/changes a value in Add New Month, frontend formulas immediately recalculate dependent cells and the UI updates instantly.

DO NOT modify or bypass this existing calculation/recompute flow.

The fix must ONLY make the Top Strip display the already-calculated Consumer Finance percentage.

ROOT CAUSE:
In the Top Strip render around L3512-3601, `effectiveVal` is currently calculated from:

    pendingVal !== undefined ? pendingVal : t.value

but the display expressions independently use:

    pendingVal !== undefined ? pendingVal : t.value

Therefore the Top Strip does not use the Consumer Finance percentage override that the middle panel already uses.

SAFE FIX:

1. Change:

    const effectiveVal = pendingVal !== undefined ? pendingVal : t.value;

to:

    let effectiveVal = pendingVal !== undefined ? pendingVal : t.value;

2. Immediately after that, add the EXISTING Consumer Finance override logic:

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
`pendingVal` must have priority.

If a user is editing/entering a value in Add New Month and `pendingVal` exists, DO NOT replace it with the override.

This is critical to preserve existing live-edit behavior.

3. In the Top Strip `displayText`, replace ONLY the inline:

    pendingVal !== undefined ? pendingVal : t.value

with:

    effectiveVal

Do this for:
- YTD PBT formatting
- Inventory Turn / A/R Turn Days formatting
- renderTopStripValue()

Do not change the formatting functions themselves.

4. Do the same in the read-only Top Strip `<span>`.

The six existing inline display occurrences identified previously should use `effectiveVal`.

DO NOT modify the middle-panel calculation.

DO NOT modify `computeConsumerFinancePercentOverride()`.

DO NOT modify the formula/recompute loop.

DO NOT modify pending edit handling.

DO NOT modify Add New Month.

DO NOT modify t.value generation.

DO NOT add any clobber guard.

DO NOT change MetricPoint type.

DO NOT change backend.

EXPECTED RESULT:

A) Cash Collections:
    Principal N/R -> 5.82% in middle panel AND Summary Top Strip
    Gross N/R    -> 4.50% in middle panel AND Summary Top Strip

B) 60+ DPD:
    Principal N/R -> 4.88% in middle panel AND Summary Top Strip
    Gross N/R    -> 3.51% in middle panel AND Summary Top Strip

C) Changing Cash Collections dropdown must NOT change 60+ DPD.

D) Changing 60+ DPD dropdown must NOT change Cash Collections.

E) Existing Add New Month/live formula calculation must behave exactly as before.

F) Existing pending/user-entered values must always win over the display override.

SCOPE:
Only:
frontend/src/app/blackbook/edit/page.tsx

BEFORE APPLYING:
- Show me the complete unified diff.
- Do NOT modify the file yet.
- Do NOT run build.
- Do NOT run tests.
- Wait for my confirmation.

First inspect the existing code and show the diff only.

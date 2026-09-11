Read-only. No modifications made. Let me confirm the exact on-disk state so the report is accurate.

Confirmed — nothing was modified this turn. Verified on-disk state:

| Location | State |
|---|---|
| L1960 }, [isConsumerFinance, latestPoint]); | **Applied** (two turns ago) |
| L1912-1944 stored-selector seed block | **Applied** (two turns ago) |
| if (!latestPoint) guard — Hunk 1 | **Not applied** |
| L3547 const effectiveVal = pendingVal !== undefined ? pendingVal : t.value; | **Not applied** (still const, no override) |
| L3529/3554/3555 + L3623/3625/3626 display inline expressions | **Not applied** (still inline) |
| L4191-4197 middle-panel override | Pre-existing, untouched |

## The finding, restated

The stored-selector seed currently on disk is **dead code**. `isConsumerFinance` becomes true from the `industry` query param on first render, while `series` is empty and `latestPoint` is null. The effect therefore runs with `lv = {}`, all three selectors read as `""`, `norm("")` returns `def`, and all six labels are written with the Principal default. When the row later arrives and the effect re-runs via the `latestPoint` dependency, `if (!next[lbl])` is false for every label, so the customer's real stored basis is never applied.

Net effect today: the ~7-11% Gross customers still display Principal, exactly as before my change. The change is inert rather than harmful.

## Why the dependency stays

To your earlier concern — `latestPoint` in the deps **cannot** overwrite a manual selection. The `if (!next[lbl])` guard short-circuits any label that already holds a value, regardless of origin. Removing the dependency would make the stored basis permanently unreachable, so the dependency is necessary; it just isn't sufficient without the `!latestPoint` early return.

## Outstanding work, all still unapplied

1. **Hunk 1** — `if (!latestPoint) { setPrincipalGrossOptions(options); return; }` after `if (cancelled) return;`. Makes the seed actually function.
2. **Hunks 2-3** — Top Strip `let effectiveVal` + CF percent override gated on `pendingVal === undefined`, then use `effectiveVal` in the inline display expressions.
3. **Not in scope, still recommended** — `setPrincipalGrossByLabel({})` in the `[name]` reset effect (L544-548). Once Hunk 1 makes seeding real, CF customer A → CF customer B navigation will leak A's basis onto B, because the component re-renders without remounting and the guard blocks re-seeding.

Two caveats on your stated expectations, unchanged from my last message: there is **no Net C/O % tile in the Top Strip** (only `Net C/O $`, `YTD Net C/O $`, `TTM Net C/O %`), so expectation #4 has nothing to synchronize; and **TTM Net C/O %** correctly will not follow the dropdown, since `perNetChargeOffTTM` has no selector in legacy.

Standing by — tell me which hunks to apply.

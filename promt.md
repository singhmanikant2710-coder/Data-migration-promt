SINGLE-FILE, BOUNDED EDIT. frontend/src/app/blackbook/edit/page.tsx. Show unified diff BEFORE applying.

FIX (Option b — clobber guard): The recompute loop overwrites per60DPD/perCashCollections/perNetChargeOff with a wrong-basis (Gross, since selector string is absent) value — often 0 or wrong — destroying the server's correct per-customer basis value. Guard the assignment so a computed value does NOT overwrite a non-zero server value when the computed is 0, OR more safely: skip overwriting the three selector-driven percent metrics entirely (let the server's correct value stand).

There are THREE copies of the loop: latestPointComputed (~L1253), rolling24WithEdits (~L1330), seriesWithEdits (~L1400). Apply to ALL THREE identically.

Current (each copy):
    for (const [key, fn] of Object.entries(tblMainCalcs)) {
        try {
            const val = (fn as (i: TblMainInputs) => number)(inputs);
            if (val != null && Number.isFinite(val)) {
                (baseValues as any)[key] = val;
            }
        } catch { }
    }

Change to add a guard: for the three selector-driven percent keys (perCashCollections, per60DPD, perNetChargeOff), do NOT overwrite a non-zero existing (server) value with the calc result (since the calc can't see the selector string and would use the wrong Gross basis). For all other keys, keep current behavior.

    const selectorDrivenPercentKeys = new Set(["perCashCollections", "per60DPD", "perNetChargeOff"]);
    for (const [key, fn] of Object.entries(tblMainCalcs)) {
        try {
            const val = (fn as (i: TblMainInputs) => number)(inputs);
            if (val != null && Number.isFinite(val)) {
                // For selector-driven percents, the calc can't see the basis selector string
                // (numeric-only values bag), so it would compute a wrong-basis value.
                // Don't let it clobber a non-zero server value that already has the correct basis.
                if (selectorDrivenPercentKeys.has(key)) {
                    const existing = toNumberLike((baseValues as any)[key]);
                    if (existing !== null && existing !== 0) {
                        continue; // keep server's correct-basis value
                    }
                }
                (baseValues as any)[key] = val;
            }
        } catch { }
    }

Apply to all THREE loop copies identically. Confirm toNumberLike is in scope in all three.

VERIFY BEFORE SHOWING DIFF:
a) Guard added for perCashCollections/per60DPD/perNetChargeOff — skips overwrite when server value is non-zero.
b) Other keys unchanged (still overwrite for live edits).
c) All three loop copies updated identically.
d) toNumberLike available in each scope.

Show the unified diff for all three. Apply nothing until I confirm.

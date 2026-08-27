SINGLE-FILE, BOUNDED EDIT. Only edit computeConsumerFinancePercentOverride in frontend/src/app/blackbook/edit/page.tsx. Show unified diff BEFORE applying.

CONTEXT: For BHG, Gross prior-month data is 0 (no Gross data). When the user switches the dropdown to "Gross N/R", the denominator is 0, so the function returns null, which SKIPS the override — leaving the previous Principal value displayed (0.34%), so the dropdown appears frozen. But legacy shows 0.00% for Gross in this case (0.34% Principal, Gross NR 0.00%). So when the denominator is 0/missing, we should return 0 (display 0.00%), NOT null (which keeps the stale value).

FIX: In the final guard, separate the "numerator missing" case from the "denominator zero/missing" case:
- If numerator (nn) is null → return null (no data at all, keep as-is).
- If denominator (dd) is null or 0 → return 0 (legacy shows 0.00% when the basis has no data).

Change the current line:
    const nn = toNumberLike(num as any);
    const dd = toNumberLike(denom as any);
    if (nn === null || dd === null || dd === 0) return null;
    return (nn / dd) * 100;

To:
    const nn = toNumberLike(num as any);
    const dd = toNumberLike(denom as any);
    if (nn === null) return null;
    if (dd === null || dd === 0) return 0;   // legacy shows 0.00% when the selected basis has no data
    return (nn / dd) * 100;

Only this change. Nothing else in the function.

VERIFY:
a) nn === null still returns null (truly no numerator data).
b) dd === null or 0 now returns 0 (so the dropdown shows 0.00% for a basis with no data, like legacy).
c) Normal case (both present) unchanged.

Show the unified diff. Apply nothing until I confirm.

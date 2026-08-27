Two fixes in frontend/src/app/blackbook/edit/page.tsx. Show me the result after each. 

=== FIX 1: computeConsumerFinancePercentOverride is BROKEN (missing const denom and const useGross lines — build error). Replace the ENTIRE function with this exact valid version: ===

function computeConsumerFinancePercentOverride(
  label: string,
  values: Record<string, number | string | null | undefined>,
  selMap: Record<string, string>,
  options: string[]
): number | null {
  const lbl = String(label || "");
  const base = lbl.replace(/\s*%$/, "");

  let numeratorAliases: string[] | null = null;
  let prinAliases: string[] = [];
  let grossAliases: string[] = [];

  if (/^cash collections/i.test(base)) {
    numeratorAliases = CF_CASH_COLL_DOLLAR_ALIASES;
    prinAliases = CF_PRIN_NR_PRIOR_ALIASES;
    grossAliases = CF_GROSS_NR_PRIOR_ALIASES;
  } else if (/^net\s*c\/o/i.test(base) || /charge[-\s]?off/i.test(base)) {
    numeratorAliases = CF_NET_CO_DOLLAR_ALIASES;
    prinAliases = CF_AVG_PRIN_TTM_ALIASES;
    grossAliases = CF_AVG_GROSS_TTM_ALIASES;
  } else if (/60\+?\s*dpd/i.test(base) || /dpd/i.test(base)) {
    numeratorAliases = CF_DPD60_DOLLAR_ALIASES;
    prinAliases = CF_PRIN_NR_ALIASES;
    grossAliases = CF_GROSS_NR_MONTH_ALIASES;
  } else {
    return null;
  }

  const num = pickMonthly(values as any, numeratorAliases);
  if (num === null) return null;

  const sel = cfResolveBasisSelection(base, selMap, options);
  const useGross = /gross/i.test(sel);

  const denom =
    pick(values as any, useGross ? grossAliases : prinAliases) ??
    pickMonthly(values as any, useGross ? grossAliases : prinAliases);

  const nn = toNumberLike(num as any);
  const dd = toNumberLike(denom as any);
  if (nn === null || dd === null || dd === 0) return null;
  return (nn / dd) * 100;
}

Confirm the function now has "const useGross" and "const denom" both present, exactly once each.

=== FIX 2: latestPointComputed does NOT hydrate the prior-month fields, so Cash Collections % denominator is null and the dropdown does nothing. Hydrate them. ===

In the latestPointComputed useMemo, inside the hydration block (where other cur* fields are assigned via coalesce), ADD these lines (near the curGrossNROrAR assignment):

    (baseValues as any)["curPrincipalNRPriorMonth"] = (baseValues as any)["curPrincipalNRPriorMonth"] ?? coalesce(
        (baseValues as any)["PrincipalNRPriorMonth"],
        (baseValues as any)["curPrincipalNRPriorMonth"]
    );
    (baseValues as any)["curGrossNRorARPriorMonth"] = (baseValues as any)["curGrossNRorARPriorMonth"] ?? coalesce(
        (baseValues as any)["GrossNRorARPriorMonth"],
        (baseValues as any)["curGrossNRorARPriorMonth"]
    );
    (baseValues as any)["curPrincipalNR"] = (baseValues as any)["curPrincipalNR"] ?? coalesce(
        (baseValues as any)["PrincipalNR"], (baseValues as any)["Principal N/R"]
    );
    (baseValues as any)["curAveragePrincipalNRTTM"] = (baseValues as any)["curAveragePrincipalNRTTM"] ?? (baseValues as any)["AveragePrincipalNRTTM"];
    (baseValues as any)["curAverageGrossNRTTM"] = (baseValues as any)["curAverageGrossNRTTM"] ?? (baseValues as any)["AverageGrossNRTTM"];

This ensures the raw fields the override needs (prior-month for Cash Collections, current-month principal for 60+DPD, Avg-TTM for Net C/O) are present in latestPointComputed.values.

NOTE: The hydration uses `?? coalesce(...)` — if the field already exists in the row it is kept; these lines only fill it if the row already carries it under an alias. If curPrincipalNRPriorMonth is genuinely absent from the backend row entirely, that's a separate backend/read issue — report if you cannot find it anywhere in the row values.

After both fixes, show me: (1) the complete corrected function, (2) the added hydration lines in context. Do not run build.

SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/app/blackbook/edit/page.tsx, only computeConsumerFinancePercentOverride. Show unified diff BEFORE applying. Do not run build.

CONTEXT: The dropdown MUST stay interactive (switching Principal/Gross changes the %, matching legacy). The previous change (returning null for Cash Collections % and 60+ DPD %) fixed the values but broke interactivity. Instead, keep the override for ALL THREE fields, but use the CORRECT denominator per field per legacy:
- Cash Collections % → PRIOR month: curPrincipalNRPriorMonth (Principal) / curGrossNRorARPriorMonth (Gross)
- 60+ DPD % → CURRENT month: curPrincipalNR (Principal) / curGrossNRorAR (Gross)
- Net C/O % → AVG TTM: curAveragePrincipalNRTTM (Principal) / curAverageGrossNRTTM (Gross)  [unchanged]

REPLACE the current function body's branching + denominator logic. The function currently (after the previous edit) only handles Net C/O %. Restore all three fields AND pick the denominator by field.

Set numeratorAliases AND a per-field denominator basis. Change the branching to:

    let numeratorAliases: string[] | null = null;
    // denominator alias sets chosen per field + per Principal/Gross selection
    let prinAliases: string[] = [];
    let grossAliases: string[] = [];

    if (/^cash collections/i.test(base)) {
        numeratorAliases = CF_CASH_COLL_DOLLAR_ALIASES;
        prinAliases = CF_PRIN_NR_PRIOR_ALIASES;      // curPrincipalNRPriorMonth
        grossAliases = CF_GROSS_NR_PRIOR_ALIASES;    // curGrossNRorARPriorMonth
    } else if (/^net\s*c\/o/i.test(base) || /charge[-\s]?off/i.test(base)) {
        numeratorAliases = CF_NET_CO_DOLLAR_ALIASES;
        prinAliases = CF_AVG_PRIN_TTM_ALIASES;       // curAveragePrincipalNRTTM
        grossAliases = CF_AVG_GROSS_TTM_ALIASES;     // curAverageGrossNRTTM
    } else if (/60\+?\s*dpd/i.test(base) || /dpd/i.test(base)) {
        numeratorAliases = CF_DPD60_DOLLAR_ALIASES;
        prinAliases = CF_PRIN_NR_ALIASES;            // curPrincipalNR (current month)
        grossAliases = CF_GROSS_NR_MONTH_ALIASES;    // curGrossNRorAR (current month)
    } else {
        return null;
    }

    const num = pickMonthly(values as any, numeratorAliases);
    if (num === null) return null;

    const sel = cfResolveBasisSelection(base, selMap, options);
    const useGross = /gross/i.test(sel);
    const denom = pick(values as any, useGross ? grossAliases : prinAliases)
        ?? pickMonthly(values as any, useGross ? grossAliases : prinAliases);

    const nn = toNumberLike(num as any);
    const dd = toNumberLike(denom as any);
    if (nn === null || dd === null || dd === 0) return null;
    return (nn / dd) * 100;

IMPORTANT — alias constants:
- Use existing constants where they exist: CF_CASH_COLL_DOLLAR_ALIASES, CF_NET_CO_DOLLAR_ALIASES, CF_DPD60_DOLLAR_ALIASES, CF_AVG_PRIN_TTM_ALIASES, CF_AVG_GROSS_TTM_ALIASES, CF_PRIN_NR_ALIASES, CF_GROSS_NR_MONTH_ALIASES.
- For the PRIOR-month aliases, check if constants exist. If CF_PRIN_NR_PRIOR_ALIASES / CF_GROSS_NR_PRIOR_ALIASES do NOT already exist, ADD them near the other CF_* alias declarations:
    const CF_PRIN_NR_PRIOR_ALIASES = ["curPrincipalNRPriorMonth", "PrincipalNRPriorMonth"];
    const CF_GROSS_NR_PRIOR_ALIASES = ["curGrossNRorARPriorMonth", "GrossNRorARPriorMonth"];

VERIFY BEFORE SHOWING DIFF:
a) All three fields (Cash Collections %, Net C/O %, 60+ DPD %) now return a computed override (interactive).
b) Denominators are field-specific: Cash Collections→PriorMonth, 60+DPD→CurrentMonth, Net C/O→AvgTTM.
c) Report whether CF_PRIN_NR_PRIOR_ALIASES / CF_GROSS_NR_PRIOR_ALIASES already existed or were added.
d) Confirm CF_PRIN_NR_ALIASES maps to curPrincipalNR (current month) and CF_GROSS_NR_MONTH_ALIASES to curGrossNRorAR (current month) — quote their definitions so we're sure 60+DPD uses current month.

Show the unified diff. Apply nothing until I confirm.

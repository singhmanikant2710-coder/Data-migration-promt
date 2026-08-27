Apply the change to computeConsumerFinancePercentOverride in frontend/src/app/blackbook/edit/page.tsx, but ensure valid syntax (the preview diff had a broken brace and a "..." placeholder — do NOT reproduce those). 

STEP 1: Add these two constants near the other CF_* alias declarations:
    const CF_PRIN_NR_PRIOR_ALIASES = ["curPrincipalNRPriorMonth", "PrincipalNRPriorMonth"];
    const CF_GROSS_NR_PRIOR_ALIASES = ["curGrossNRorARPriorMonth", "GrossNRorARPriorMonth"];

STEP 2: Also FIX the existing CF_GROSS_NR_MONTH_ALIASES to include the real column name curGrossNRorAR (the current one lacks it, so Gross 60+DPD would find no denominator). Change:
    const CF_GROSS_NR_MONTH_ALIASES = ["curGrossNR", "GrossNR", "Gross N/R", "Gross"];
to:
    const CF_GROSS_NR_MONTH_ALIASES = ["curGrossNRorAR", "curGrossNR", "GrossNR", "Gross N/R", "Gross"];

STEP 3: Replace the ENTIRE body of computeConsumerFinancePercentOverride (from the first line after the signature to the closing brace) with exactly this — as ONE valid function, no stray braces, no "..." :

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

Make sure the function has exactly ONE opening brace (after the signature) and ONE closing brace at the end. No code outside the function. No "..." placeholders.

After applying, show me the COMPLETE function from signature to closing brace, plus the 3 alias constant lines, so I can verify valid syntax and correct denominators.

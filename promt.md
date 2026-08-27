SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/app/blackbook/edit/page.tsx, only the function computeConsumerFinancePercentOverride. Show unified diff BEFORE applying. Do not run build.

BUG: computeConsumerFinancePercentOverride overrides Cash Collections %, Net C/O %, and 60+ DPD % using Avg-TTM denominators (curAveragePrincipalNRTTM / curAverageGrossNRTTM). This is CORRECT only for Net C/O % (legacy perNetChargeOff uses Avg TTM). It is WRONG for:
- Cash Collections % (legacy uses curPrincipalNRPriorMonth / curGrossNRorARPriorMonth = PRIOR month)
- 60+ DPD % (legacy uses curPrincipalNR / curGrossNRorAR = CURRENT month)

The backend-stored perCashCollections and per60DPD are ALREADY correct (legacy-match). So the fix is: skip the override for Cash Collections % and 60+ DPD %, letting the tile fall back to its correct stored value. Keep the override ONLY for Net C/O % (where Avg TTM is correct).

FIX: In computeConsumerFinancePercentOverride, change the label branching so that ONLY Net C/O % returns a computed override; Cash Collections % and 60+ DPD % return null (skip override, use stored value).

Current branching:
    if (/^cash collections/i.test(base)) {
        numeratorAliases = CF_CASH_COLL_DOLLAR_ALIASES;
    } else if (/^net\s*c\/o/i.test(base) || /charge[-\s]?off/i.test(base)) {
        numeratorAliases = CF_NET_CO_DOLLAR_ALIASES;
    } else if (/60+\s*dpd/i.test(base) || /dpd/i.test(base)) {
        numeratorAliases = CF_DPD60_DOLLAR_ALIASES;
    } else {
        return null;
    }

Change to (only Net C/O % is overridden; the others return null so the correct stored value is used):
    if (/^net\s*c\/o/i.test(base) || /charge[-\s]?off/i.test(base)) {
        numeratorAliases = CF_NET_CO_DOLLAR_ALIASES;
    } else {
        // Cash Collections % (prior-month) and 60+ DPD % (current-month) are already
        // computed correctly by the backend/tblMainCalcs stored value; do NOT override them
        // with Avg-TTM denominators. Only Net C/O % legitimately uses Avg TTM here.
        return null;
    }

Do NOT change anything else in the function (the denominator logic below stays, since it's now only reached for Net C/O %, where Avg TTM is correct). Do NOT change the call site.

VERIFY BEFORE SHOWING DIFF:
a) Only Net C/O % now produces an override; Cash Collections % and 60+ DPD % return null.
b) The Avg-TTM denominator logic is unchanged (only reached for Net C/O %).
c) No other code changed.

Show the unified diff. Apply nothing until I confirm.

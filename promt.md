SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only the perCashCollections computation block. Show unified diff BEFORE applying. Do not run build.

BUG: The backend perCashCollections uses curPrincipalNR / curGrossNRorAR (CURRENT month) as the denominator. But the legacy formula and the frontend both use the PRIOR month: curPrincipalNRPriorMonth / curGrossNRorARPriorMonth. This makes Cash Collections % wrong/0.

Legacy: perCashCollections = curCashCollections / curPrincipalNRPriorMonth (if Principal selection) else / curGrossNRorARPriorMonth.

FIX: In the perCashCollections block, change the denominator columns from current-month to prior-month:
- curPrincipalNR  ->  curPrincipalNRPriorMonth
- curGrossNRorAR  ->  curGrossNRorARPriorMonth

The block currently reads (roughly):
    if (Num("perCashCollections") && Has("curCashCollections"))
    {
        string denCash;
        if (Has("curPrincipalNR") && Has("curGrossNRorAR"))
        {
            denCash = selCash is not null
                ? $"(CASE WHEN {selCash} LIKE 'PRINCIPAL%' THEN NULLIF(CONVERT(decimal(19,6), m.[curPrincipalNR]),0) ELSE NULLIF(CONVERT(decimal(19,6), m.[curGrossNRorAR]),0) END)"
                : "NULLIF(CONVERT(decimal(19,6), m.[curGrossNRorAR]),0)";
        }
        else if (Has("curPrincipalNR"))
        {
            denCash = "NULLIF(CONVERT(decimal(19,6), m.[curPrincipalNR]),0)";
        }
        else
        {
            denCash = "NULLIF(CONVERT(decimal(19,6), m.[curGrossNRorAR]),0)";
        }
        ...
    }

Change it to use the PRIOR-MONTH columns and check for their existence:
- The Has(...) checks: "curPrincipalNR" -> "curPrincipalNRPriorMonth", "curGrossNRorAR" -> "curGrossNRorARPriorMonth"
- The column references m.[curPrincipalNR] -> m.[curPrincipalNRPriorMonth], m.[curGrossNRorAR] -> m.[curGrossNRorARPriorMonth]
- Keep the CASE/selCash logic, the NULLIF, the CONVERT, and the final sets.Add(...) numerator (m.[curCashCollections]) exactly the same. Only the denominator columns change to their PriorMonth equivalents.

VERIFY BEFORE SHOWING DIFF:
a) Confirm columns curPrincipalNRPriorMonth and curGrossNRorARPriorMonth exist in tblMain (they do per our data). If a Has() check is needed, use the PriorMonth column names.
b) Numerator stays m.[curCashCollections]. Only denominator changed to PriorMonth.
c) No other calc block changed.

Show the unified diff. Apply nothing until I confirm.

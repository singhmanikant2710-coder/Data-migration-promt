SINGLE-FILE, BOUNDED EDIT. Only edit backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs, only the perCashCollections block. Show unified diff BEFORE applying. Do not run build.

BUG: The CASE uses `WHEN {selCash} LIKE 'PRINCIPAL'` (no wildcard). The selection value is 'Principal N/R', and 'Principal N/R' LIKE 'PRINCIPAL' is FALSE. So the Principal branch never matches and it uses the wrong denominator. Legacy selects Principal when selection = "Principal N/R".

FIX: Add a % wildcard so it matches any value starting with PRINCIPAL. In the perCashCollections block, change:

    ? $@"(CASE WHEN {selCash} LIKE 'PRINCIPAL' THEN NULLIF(CONVERT(decimal(19,6), m.[curPrincipalNRPriorMonth]),0)

to:

    ? $@"(CASE WHEN {selCash} LIKE 'PRINCIPAL%' THEN NULLIF(CONVERT(decimal(19,6), m.[curPrincipalNRPriorMonth]),0)

Only add the % after PRINCIPAL in that one LIKE pattern. Change nothing else in the block — denominator columns stay curPrincipalNRPriorMonth/curGrossNRorARPriorMonth, numerator stays curCashCollections.

ALSO REPORT (do not change yet): search the whole file for other occurrences of LIKE 'PRINCIPAL' (without %) in selection-based blocks like perNetChargeOff, per60DPD, perCashCollections. List each occurrence with its line/context, since they likely have the same bug (selection "Principal N/R" won't match 'PRINCIPAL').

VERIFY BEFORE SHOWING DIFF:
a) Only the perCashCollections LIKE pattern got the % wildcard.
b) List all other LIKE 'PRINCIPAL' (no %) found in the file.

Show the unified diff. Apply nothing until I confirm.

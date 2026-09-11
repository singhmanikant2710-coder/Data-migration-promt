SELECT 
    strPrincipalOrGrossCalculationSelectionCashCollection AS cash_sel,
    COUNT(*) AS cnt
FROM tblMain
WHERE strPrincipalOrGrossCalculationSelectionCashCollection IS NOT NULL
  AND LTRIM(RTRIM(strPrincipalOrGrossCalculationSelectionCashCollection)) <> ''
GROUP BY strPrincipalOrGrossCalculationSelectionCashCollection;

SELECT 
    strPrincipalOrGrossCalculationSelectionper60DPD AS dpd_sel,
    COUNT(*) AS cnt
FROM tblMain
WHERE strPrincipalOrGrossCalculationSelectionper60DPD IS NOT NULL
  AND LTRIM(RTRIM(strPrincipalOrGrossCalculationSelectionper60DPD)) <> ''
GROUP BY strPrincipalOrGrossCalculationSelectionper60DPD;

-- Exact rows dhundho jo delete karne hain
SELECT anmMain, strCustomerName, strMonthKey, intFiscalYear
FROM tblMain
WHERE strMonthKey IN ('202701','202612','202611','202610');

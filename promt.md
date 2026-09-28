SELECT strMonthKey, cur60DPD, per60DPD, curCashCollections, curNetChargeOff,
       curNetChargeOffYTD, curNetChargeOffTTM, curDiscountDividedByReserve,
       perDiscountDividedByReserve, perReserveCoverage
FROM tblMain
WHERE strCustomerName = 'AMERICAN CREDIT ACCEPTANCE' AND strMonthKey = '<MK>';


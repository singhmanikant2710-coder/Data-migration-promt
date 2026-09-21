SELECT strCustomerName, strMonthKey, strCovenantName,
       strThreshold1, strThreshold2, strThreshold3, strThreshold4, strThreshold5, strThreshold6,
       strActual1, strActual2, strActual3, strActual4, strActual5, strActual6
FROM tblMainDisplayCovenants
WHERE strCustomerName LIKE '%ATHENS%';

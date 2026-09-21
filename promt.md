SELECT strMonthKey,
  strCovenantName1, dblCovenantActual1,
  strCovenantName2, dblCovenantActual2,
  strCovenantName3, dblCovenantActual3,
  strCovenantName4, dblCovenantActual4,
  strCovenantName5, dblCovenantActual5,
  strCovenantName6, dblCovenantActual6
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC' AND strMonthKey = '202510';

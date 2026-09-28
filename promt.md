SELECT strMonthKey, curFTBLine AS LineBalance
FROM tblMain
WHERE strCustomerName = 'GRACELAND RENTALS LLC'
  AND strMonthKey = '202603';

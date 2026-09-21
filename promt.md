SELECT strMonthKey, curTotalAdjustedLiabilities  -- ya jo bhi liabB-matching column hai
FROM tblMain
WHERE strCustomerName = 'ATHENS PAPER COMPANY INC' AND strMonthKey = '202510';

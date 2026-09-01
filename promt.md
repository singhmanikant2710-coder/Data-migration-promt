-- 202609 galat hai (2026/9), delete karo
DELETE FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) LIKE '%BANKERS HEALTHCARE%'
  AND strMonthKey = '202609';

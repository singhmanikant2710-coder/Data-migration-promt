SELECT strMonthKey, curProfitBeforeTaxes, curInterestExpense
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND strMonthKey <= '202607'
ORDER BY strMonthKey DESC;

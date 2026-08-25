-- New app SQL Server DB pe: dekho purane 12 months ka interest expense actually stored hai kya
SELECT TOP 24
    strMonthKey,
    strCustomerName,
    curInterestExpense,          -- monthly interest (adjust column name agar alag ho)
    curProfitBeforeTaxes,        -- monthly PBT
    curInterestExpenseTTM,       -- agar ye precomputed column exist karta hai
    curProfitBeforeTaxesTTM
FROM tblMain
WHERE LTRIM(RTRIM(strCustomerName)) = 'AMERICAN CREDIT ACCEPTANCE'
  AND LTRIM(RTRIM(strMonthKey)) <= '202607'
ORDER BY LTRIM(RTRIM(strMonthKey)) DESC;

SELECT DelinquentID, COUNT(*) 
FROM dbo.[dbo_CommercialCreditDataAcq] 
GROUP BY DelinquentID 
ORDER BY 2 DESC;

SELECT DelinquentID, COUNT(*) 
FROM dbo.[dbo_CommercialCreditDataAcq] WITH (NOLOCK)
GROUP BY DelinquentID 
ORDER BY 2 DESC;

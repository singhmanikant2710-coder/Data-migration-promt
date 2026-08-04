SELECT Review_id, Customer_name, Review_approver_name, Sample_id
FROM dbo.[02_CORE_02_Reviews]
WHERE Review_approver_name LIKE '%GEOFFREY%'
   OR Review_approver_name LIKE '%HOULDITCH%'
ORDER BY Review_id DESC;

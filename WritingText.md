SELECT TOP 5 r.Review_id
FROM dbo.[02_CORE_02_Reviews] r
WHERE NOT EXISTS (SELECT 1 FROM dbo.[02_CORE_06_Policy Exceptions] p
                  WHERE p.Review_id = r.Review_id);

UPDATE dbo.[02_CORE_02_Reviews]
SET Policy_exception_information = N'TEST STORED NOTES',
    Policy_exception_mitigation  = N'Mitigated'
WHERE Review_id = <id>;   -- 1 row affected

SELECT TOP 5 r.Review_id
FROM dbo.[02_CORE_02_Reviews] r
WHERE NOT EXISTS (SELECT 1 FROM dbo.[02_CORE_06_Policy Exceptions] p
                  WHERE p.Review_id = r.Review_id)
  AND r.Policy_exception_information IS NULL
  AND r.Policy_exception_mitigation IS NULL;


  SELECT Policy_exception_information, Policy_exception_mitigation
FROM dbo.[02_CORE_02_Reviews] WHERE Review_id = <id>;

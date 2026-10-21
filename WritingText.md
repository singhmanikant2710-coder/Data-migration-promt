SELECT TOP 5 r.Review_id, r.Sample_id, r.Locked,
       r.Policy_exception_information, r.Policy_exception_mitigation
FROM dbo.[02_CORE_02_Reviews] r
JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE ISNULL(r.Locked, 0) = 0
  AND ISNULL(s.Closed, 0) = 0
  AND ISNULL(r.Cancelled, 0) = 0
  AND ISNULL(r.Policy_exception_information, '') = ''
  AND NOT EXISTS (SELECT 1 FROM dbo.[02_CORE_06_Policy Exceptions] pe
                  WHERE pe.Review_id = r.Review_id);

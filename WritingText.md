SELECT TOP 5 f.Review_id, r.Sample_id, r.Customer_name, f.Finding_CRM_component, f.Finding_code
FROM dbo.[02_CORE_07_Findings] f
JOIN dbo.[02_CORE_02_Reviews] r ON r.Review_id = f.Review_id
JOIN dbo.[02_CORE_01_Samples] s ON s.Sample_id = r.Sample_id
WHERE ISNULL(r.Locked,0) = 0 AND ISNULL(r.Cancelled,0) = 0 AND ISNULL(s.Closed,0) = 0
  AND f.Finding_code LIKE 'UW-%';

SELECT f1.Review_id, r.Customer_name, r.Sample_id,
       f1.Finding_code AS Code_1, f2.Finding_code AS Code_2,
       LEFT(CAST(f1.Finding_comments AS nvarchar(max)), 120) AS Comments
FROM dbo.[02_CORE_07_Findings] f1 WITH (NOLOCK)
JOIN dbo.[02_CORE_07_Findings] f2 WITH (NOLOCK)
  ON f2.Review_id = f1.Review_id AND f2.Finding_code > f1.Finding_code
 AND CAST(f2.Finding_comments AS nvarchar(max)) = CAST(f1.Finding_comments AS nvarchar(max))
JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.Review_id = f1.Review_id
WHERE LEN(LTRIM(RTRIM(CAST(f1.Finding_comments AS nvarchar(max))))) > 0
ORDER BY f1.Review_id;

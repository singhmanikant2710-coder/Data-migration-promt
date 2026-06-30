-- Library me finding code ka category aur description dekho
SELECT [Finding_code], [Finding_CRM_component], [Finding_category], 
       [Finding_description], [Finding_guidance]
FROM dbo.[03_LIBRARY_01_CAS Findings] WITH (NOLOCK)
WHERE [Finding_code] IN ('CS-101','SS-101','UW-103')
ORDER BY [Finding_code];

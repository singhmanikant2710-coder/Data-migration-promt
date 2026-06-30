-- Form pe Component/Code blank dikhe wale codes library me hain ya nahi?
SELECT [Finding_CRM_component], [Finding_code], [Active]
FROM dbo.[03_LIBRARY_01_CAS Findings] WITH (NOLOCK)
WHERE [Finding_code] IN ('CS-101','CS-103','LA-101','SM-101','SM-103','SM-104',
                         'SS-101','SS-102','SS-103','UW-103','UW-109','UW-110','UW-112')
ORDER BY [Finding_code];

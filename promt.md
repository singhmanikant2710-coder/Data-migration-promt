SELECT 
    LTRIM(RTRIM([Finding_CRM_component])) AS Component,
    LTRIM(RTRIM([Finding_code])) AS Finding_code,
    LTRIM(RTRIM([Finding_category])) AS Category
FROM dbo.[03_LIBRARY_01_CAS_Findings]
ORDER BY [Finding_CRM_component], [Finding_code];

SELECT 
    LTRIM(RTRIM([Finding_code])) AS Finding_code,
    COUNT(DISTINCT LTRIM(RTRIM([Finding_CRM_component]))) AS component_count
FROM dbo.[03_LIBRARY_01_CAS_Findings]
GROUP BY LTRIM(RTRIM([Finding_code]))
HAVING COUNT(DISTINCT LTRIM(RTRIM([Finding_CRM_component]))) > 1;

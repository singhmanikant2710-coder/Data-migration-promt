SELECT DISTINCT [Finding_CRM_component]
FROM dbo.[02_CORE_07_Findings] WITH (NOLOCK)
WHERE [Finding_CRM_component] LIKE '06%'
   OR [Finding_CRM_component] LIKE '01%'
ORDER BY [Finding_CRM_component];

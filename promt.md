-- Library table (jisse backend dropdown options laata hai)
SELECT DISTINCT [Finding_CRM_component] AS lib_component
FROM dbo.[03_LIBRARY_01_CAS Findings] WITH (NOLOCK)
ORDER BY [Finding_CRM_component];

-- Transactional table (jisse saved findings aate hain)
SELECT DISTINCT [Finding_CRM_component] AS txn_component
FROM dbo.[02_CORE_07_Findings] WITH (NOLOCK)
ORDER BY [Finding_CRM_component];

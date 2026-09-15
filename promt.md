Findings data rows were already protected from before — only the header row wasn't, which I've now fixed too. So Findings and Observations should be fully safe now.


Find the longest Policy Exception description across all reviews, to test whether it causes clipping in the CRM Summary PDF (wrap={false} risk on the Policy Exception table). READ-ONLY.

1. Find the repository/query that loads Policy Exception details for the CRM Summary report (SqlCrmSummaryTableReportRepository.cs or similar — the one feeding the Policy Exception Information table).
2. Run (or show me the exact SQL to run) a query that finds the TOP 10 longest Exception_description values, along with their Review_id, Customer_name, and Sample_id, so I can pick a test case with real long-description data.

Report the top 5 results (Sample ID + Customer + description length).


SELECT TOP 10
    pe.[Review_id],
    r.[Customer_name],
    r.[Sample_id],
    pe.[Exception_description],
    LEN(pe.[Exception_description]) AS DescLength
FROM dbo.[02_CORE_06_Policy_Exceptions] pe
INNER JOIN dbo.[02_CORE_02_Reviews] r ON r.[Review_id] = pe.[Review_id]
WHERE pe.[Exception_description] IS NOT NULL
ORDER BY LEN(pe.[Exception_description]) DESC;


SELECT DISTINCT TABLE_NAME, COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE COLUMN_NAME LIKE '%Exception%Desc%' OR TABLE_NAME LIKE '%Policy%Exception%';

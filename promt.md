SELECT TOP 20
    c.Sample_id,
    c.Review_id,
    COUNT(*) AS TotalChecklistRows,
    SUM(CASE WHEN c.Checklist_answer IS NOT NULL AND c.Checklist_answer <> '' THEN 1 ELSE 0 END) AS RowsWithAnswer,
    COUNT(DISTINCT c.Checklist_category) AS DistinctCategories
FROM [02_CORE_08_Checklists] c
GROUP BY c.Sample_id, c.Review_id
HAVING SUM(CASE WHEN c.Checklist_answer IS NOT NULL AND c.Checklist_answer <> '' THEN 1 ELSE 0 END) > 0
ORDER BY RowsWithAnswer DESC;

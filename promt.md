SELECT TOP 20
    ch.ReviewId,
    r.Sample_id,
    r.Sample_name,
    COUNT(*) AS TotalChecklistRows,
    SUM(CASE WHEN ch.Checklist_answer IS NOT NULL AND ch.Checklist_answer <> '' THEN 1 ELSE 0 END) AS RowsWithAnswer,
    COUNT(DISTINCT ch.Checklist_category) AS DistinctCategories
FROM dbo.[02_CORE_08_Checklists] ch
INNER JOIN dbo.[02_CORE_02_Reviews] r ON r.Review_id = ch.ReviewId
GROUP BY ch.ReviewId, r.Sample_id, r.Sample_name
HAVING SUM(CASE WHEN ch.Checklist_answer IS NOT NULL AND ch.Checklist_answer <> '' THEN 1 ELSE 0 END) > 0
ORDER BY RowsWithAnswer DESC;

SELECT
    ch.ReviewId,
    ch.Checklist_category,
    ch.Checklist_question,
    ch.Checklist_guidance,
    ch.Checklist_answer,
    ch.Checklist_comments
FROM dbo.[02_CORE_08_Checklists] ch
WHERE ch.ReviewId = <PASTE_REVIEW_ID_HERE>
ORDER BY ch.Checklist_category, ch.Checklist_question;

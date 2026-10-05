SELECT * FROM dbo.[03_LIBRARY_09_Selections]
WHERE Tab = 'Samples'
ORDER BY Section, Selection_id;

SELECT Sample_type, COUNT(*) AS Cnt FROM dbo.[02_CORE_01_Samples] GROUP BY Sample_type;
SELECT Sample_target, COUNT(*) AS Cnt FROM dbo.[02_CORE_01_Samples] GROUP BY Sample_target;

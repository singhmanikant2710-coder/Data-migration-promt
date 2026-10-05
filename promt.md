SELECT Sample_id, Sample_name, Sample_type, Sample_target
FROM dbo.[02_CORE_01_Samples]
WHERE Sample_id IN (363, 364, 375);

SELECT Sample_type, COUNT(*) AS Cnt FROM dbo.[02_CORE_01_Samples] GROUP BY Sample_type;

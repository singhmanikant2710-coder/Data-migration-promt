-- History dropdown jo "finalized reviews wale samples" maangta hai,
-- agar wo Samples.Sample_id pe join karta hai, ye 0 dega (= khali dropdown)
SELECT COUNT(*) AS matched_via_direct_join
FROM dbo.[02_CORE_01_Samples] s
WHERE EXISTS (
  SELECT 1 FROM dbo.[02_CORE_02_Reviews] r
  WHERE r.Sample_id = s.Sample_id
    AND r.Review_finalized_date IS NOT NULL
    AND (r.Cancelled IS NULL OR r.Cancelled = 0)
);

-- Aur ye dikhayega ki name-parse wala mapping kaam karta hai (= dropdown bhar jana chahiye)
SELECT COUNT(*) AS matched_via_name_parse
FROM dbo.[02_CORE_01_Samples] s
WHERE EXISTS (
  SELECT 1 FROM dbo.[02_CORE_02_Reviews] r
  WHERE r.Sample_id = TRY_CAST(LEFT(s.Sample_name, CHARINDEX(' -', s.Sample_name + ' -') - 1) AS INT)
    AND r.Review_finalized_date IS NOT NULL
    AND (r.Cancelled IS NULL OR r.Cancelled = 0)
);

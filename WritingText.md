SELECT COUNT(*) AS Total,
       SUM(CASE WHEN a.Facility_number_calc IS NOT NULL THEN 1 ELSE 0 END) AS FacilityFilled
FROM dbo.[02_CORE_04_Accounts] a WITH (NOLOCK)
JOIN dbo.[02_CORE_02_Reviews] r WITH (NOLOCK) ON r.Review_id = a.Review_id
WHERE ISNULL(r.Cancelled,0) = 0
  AND ((a.CAS_PD IS NOT NULL AND (a.Bank_PD IS NULL OR a.CAS_PD <> a.Bank_PD))
    OR (a.CAS_LGD IS NOT NULL AND (a.Bank_LGD IS NULL OR a.CAS_LGD <> a.Bank_LGD)));

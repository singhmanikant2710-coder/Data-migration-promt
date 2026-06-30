-- Transactional table me jo description saved hai vs library ka description — same hai ya alag?
SELECT t.[Finding_code], 
       t.[Finding_description] AS txn_description,
       l.[Finding_description] AS lib_description
FROM dbo.[02_CORE_07_Findings] t
JOIN dbo.[03_LIBRARY_01_CAS Findings] l ON t.[Finding_code] = l.[Finding_code]
WHERE t.[Review_id] = 12634;

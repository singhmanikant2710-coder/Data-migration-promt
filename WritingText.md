UPDATE dbo.[02_CORE_01_Samples]
SET [Closed_Date] = '2018-10-30'
WHERE [Sample_id] = 384 AND [Sample_name] = N'RR_TEST_08_CLOSED_ON_CUTOFF';


UPDATE dbo.[02_CORE_01_Samples]
SET [Closed] = 0
WHERE [Sample_id] = 384 AND [Sample_name] = N'RR_TEST_08_CLOSED_ON_CUTOFF';

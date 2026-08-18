SELECT [Review_approval_date], [Review finalized date], 
       [Review_initial_approval_date], [Locked]
FROM dbo.[02_CORE_02_Reviews] WHERE [Review_id] = X;
-- expect: approval NULL, finalized NULL, initial set, Locked 0

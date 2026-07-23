SELECT [Review_id], [Customer_name], [CRO_name], [CRO_manager_name],
       [Relationship_mgr_name], [Portfolio_mgr_name], [Review_approver_name]
FROM dbo.[02_CORE_02_Reviews] WITH (NOLOCK)
WHERE [Review_id] = 21882;

-- Test ke liye eligible reviews dhundo (Unlock button in par dikhega)
SELECT TOP 10
    r.[Review_id],
    r.[Sample_id],
    r.[Review_approval_date],
    r.[Review_initial_approval_date],
    r.[Review finalized date],
    r.[Review_approver_name],
    r.[Locked]
FROM dbo.[02_CORE_02_Reviews] r
WHERE r.[Locked] = 1                          -- Locked
  AND r.[Review_approval_date] IS NOT NULL    -- approval set (button visible condition)
  AND r.[Review finalized date] IS NOT NULL   -- finalized set (taaki clear dikhe)
  AND (r.[Cancelled] IS NULL OR r.[Cancelled] = 0)
ORDER BY r.[Review_id] DESC;


SELECT 
    r.[Review_id],
    r.[Sample_id],
    r.[ECIF],           -- ya jo bhi customer identifier column hai
    r.[Borrower_name]   -- ya customer name
FROM dbo.[02_CORE_02_Reviews] r
WHERE r.[Review_id] = <PICKED_REVIEW_ID>;


-- BEFORE unlock — ye values note karo
SELECT 
    [Review_id],
    [Review_approval_date]         AS Before_Approval,
    [Review_initial_approval_date] AS Before_InitialApproval,
    [Review finalized date]        AS Before_Finalized,
    [Review_approver_name]         AS Before_Approver,
    [Locked]                       AS Before_Locked
FROM dbo.[02_CORE_02_Reviews]
WHERE [Review_id] = <PICKED_REVIEW_ID>;


-- AFTER General Revisions unlock
SELECT 
    [Review_id],
    [Review_approval_date]         AS After_Approval,      -- expect: NULL (cleared)
    [Review_initial_approval_date] AS After_InitialApproval,-- expect: old approval value (first unlock)
    [Review finalized date]        AS After_Finalized,      -- expect: NULL (cleared) ← FIX 2
    [Review_approver_name]         AS After_Approver,       -- expect: SAME as before
    [Locked]                       AS After_Locked          -- expect: 0 (FALSE)
FROM dbo.[02_CORE_02_Reviews]
WHERE [Review_id] = <PICKED_REVIEW_ID>;


-- AFTER Reconsideration unlock
SELECT 
    [Review_id],
    [Review_approval_date]         AS After_Approval,      -- expect: NULL (cleared) ← FIX 3
    [Review_initial_approval_date] AS After_InitialApproval,-- expect: old approval (first unlock) ← FIX 3
    [Review finalized date]        AS After_Finalized,      -- expect: NULL (cleared) ← FIX 3
    [Review_approver_name]         AS After_Approver,       -- expect: SAME
    [Locked]                       AS After_Locked,         -- expect: 0
    [Reconsideration]              AS Reconsideration,       -- expect: saved value
    [Reconsideration_date]         AS Recon_Date,            -- expect: saved
    [Reconsideration_description]  AS Recon_Desc             -- expect: saved
FROM dbo.[02_CORE_02_Reviews]
WHERE [Review_id] = <NEW_REVIEW_ID>;



-- AFTER Appeal unlock
SELECT 
    [Review_id],
    [Review_approval_date]         AS After_Approval,      -- expect: NULL ← FIX 3
    [Review_initial_approval_date] AS After_InitialApproval,-- expect: old approval ← FIX 3
    [Review finalized date]        AS After_Finalized,      -- expect: NULL ← FIX 3
    [Review_approver_name]         AS After_Approver,       -- expect: SAME
    [Locked]                       AS After_Locked,         -- expect: 0
    [Appeal]                       AS Appeal,                -- expect: saved
    [Appeal_date]                  AS Appeal_Date,           -- expect: saved
    [Appeal_description]           AS Appeal_Desc            -- expect: saved
FROM dbo.[02_CORE_02_Reviews]
WHERE [Review_id] = <NEW_REVIEW_ID_2>;


-- Column names dekho
SELECT COLUMN_NAME 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE TABLE_NAME = '02_CORE_02_Reviews'
  AND (COLUMN_NAME LIKE '%Reconsideration%' 
       OR COLUMN_NAME LIKE '%Appeal%'
       OR COLUMN_NAME LIKE '%ECIF%'
       OR COLUMN_NAME LIKE '%finalized%');

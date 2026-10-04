DECLARE @j NVARCHAR(MAX) = N'<Step 1 ka JSON yahan paste>';

IF NOT EXISTS (SELECT 1 FROM dbo.[<HelpTipsTable>]
               WHERE Help_tip_form = '04_REVIEW FORM_04'
                 AND Help_tip_topic = 'Checklist Questions')
BEGIN
    IF COLUMNPROPERTY(OBJECT_ID('dbo.[<HelpTipsTable>]'), 'Help_tip_id', 'IsIdentity') = 1
        INSERT INTO dbo.[<HelpTipsTable>] (Help_tip_form, Help_tip_topic, Help_tip)
        SELECT Help_tip_form, Help_tip_topic, Help_tip
        FROM OPENJSON(@j) WITH (Help_tip_form NVARCHAR(255), Help_tip_topic NVARCHAR(255), Help_tip NVARCHAR(MAX));
    ELSE
        INSERT INTO dbo.[<HelpTipsTable>] (Help_tip_id, Help_tip_form, Help_tip_topic, Help_tip)
        SELECT (SELECT ISNULL(MAX(Help_tip_id),0) + 1 FROM dbo.[<HelpTipsTable>]),
               Help_tip_form, Help_tip_topic, Help_tip
        FROM OPENJSON(@j) WITH (Help_tip_form NVARCHAR(255), Help_tip_topic NVARCHAR(255), Help_tip NVARCHAR(MAX));
END

SELECT Help_tip_id, Help_tip_form, Help_tip_topic, LEFT(Help_tip, 100) AS Preview
FROM dbo.[<HelpTipsTable>] ORDER BY Help_tip_id;

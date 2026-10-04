SELECT * FROM dbo.[<HelpTipsTable>]
WHERE Help_tp_topic = 'Checklist Questions'
FOR JSON PATH;


SELECT name, is_identity FROM sys.columns
WHERE object_id = OBJECT_ID('dbo.[<HelpTipsTable>]');

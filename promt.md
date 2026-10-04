SELECT LEN(CAST(Help_tip AS NVARCHAR(MAX))) AS TipLength,
       CONVERT(VARCHAR(MAX), CAST(CAST(Help_tip AS NVARCHAR(MAX)) AS VARBINARY(MAX)), 1) AS HexTip
FROM dbo.[03_LIBRARY_06_Help Tips]
WHERE Help_tip_topic = 'Checklist Questions';

DECLARE @tip NVARCHAR(MAX) = CAST(<HexTip yahan paste, bina quotes> AS NVARCHAR(MAX));

IF NOT EXISTS (SELECT 1 FROM dbo.[03_LIBRARY_06_Help Tips]
               WHERE Help_tip_form = '04_REVIEW FORM_04'
                 AND Help_tip_topic = 'Checklist Questions')
BEGIN
    IF COLUMNPROPERTY(OBJECT_ID('dbo.[03_LIBRARY_06_Help Tips]'), 'Help_tip_id', 'IsIdentity') = 1
        INSERT INTO dbo.[03_LIBRARY_06_Help Tips] (Help_tip_form, Help_tip_topic, Help_tip)
        VALUES ('04_REVIEW FORM_04', 'Checklist Questions', @tip);
    ELSE
        INSERT INTO dbo.[03_LIBRARY_06_Help Tips] (Help_tip_id, Help_tip_form, Help_tip_topic, Help_tip)
        SELECT ISNULL(MAX(Help_tip_id), 0) + 1, '04_REVIEW FORM_04', 'Checklist Questions', @tip
        FROM dbo.[03_LIBRARY_06_Help Tips];
END

SELECT Help_tip_id, Help_tip_topic, LEN(CAST(Help_tip AS NVARCHAR(MAX))) AS TipLength
FROM dbo.[03_LIBRARY_06_Help Tips] ORDER BY Help_tip_id;

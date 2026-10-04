-- UAT #211: Checklist Questions help tip (exact copy of QA row).
-- Idempotent: inserts only if the row does not exist. Safe for Dev, QA, Prod.

DECLARE @tip NVARCHAR(MAX) = CAST(<hex> AS NVARCHAR(MAX));

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
FROM dbo.[03_LIBRARY_06_Help Tips]
WHERE Help_tip_topic = 'Checklist Questions';

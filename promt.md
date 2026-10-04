/*
  UAT #211 - Checklist Questions help tip (Review Form > Checklist section).
  Source: exact copy of the QA row (hex-encoded to keep HTML byte-identical).
  Idempotent: inserts only if the form + topic row does not exist.
  Safe to run on Dev, QA (already exists -> no-op) and Prod.
  Verify with the SELECT at the bottom of this file.
*/

SET NOCOUNT ON;

DECLARE @Form  NVARCHAR(255) = N'04_REVIEW FORM_04';
DECLARE @Topic NVARCHAR(255) = N'Checklist Questions';

-- >>> PASTE THE QA HEX BELOW (replace <QA_HEX>, keep "CAST(" and " AS NVARCHAR(MAX));") <<<
DECLARE @HelpTip NVARCHAR(MAX) = CAST(<QA_HEX> AS NVARCHAR(MAX));

BEGIN TRY
  BEGIN TRANSACTION;

  IF NOT EXISTS (
    SELECT 1
    FROM dbo.[03_LIBRARY_06_Help Tips] WITH (UPDLOCK, HOLDLOCK)
    WHERE LTRIM(RTRIM([Help_tip_form]))  = LTRIM(RTRIM(@Form))
      AND LTRIM(RTRIM([Help_tip_topic])) = LTRIM(RTRIM(@Topic))
  )
  BEGIN
    IF COLUMNPROPERTY(OBJECT_ID('dbo.[03_LIBRARY_06_Help Tips]'), 'Help_tip_id', 'IsIdentity') = 1
      INSERT INTO dbo.[03_LIBRARY_06_Help Tips] ([Help_tip_form], [Help_tip_topic], [Help_tip])
      VALUES (@Form, @Topic, @HelpTip);
    ELSE
      INSERT INTO dbo.[03_LIBRARY_06_Help Tips] ([Help_tip_id], [Help_tip_form], [Help_tip_topic], [Help_tip])
      SELECT ISNULL(MAX([Help_tip_id]), 0) + 1, @Form, @Topic, @HelpTip
      FROM dbo.[03_LIBRARY_06_Help Tips] WITH (UPDLOCK, HOLDLOCK);
  END

  COMMIT TRANSACTION;
END TRY
BEGIN CATCH
  IF XACT_STATE() <> 0 ROLLBACK TRANSACTION;
  THROW;
END CATCH;

-- Verification: exactly one row should be returned; TipLength must match QA.
SELECT [Help_tip_id], [Help_tip_form], [Help_tip_topic],
       LEN(CAST([Help_tip] AS NVARCHAR(MAX))) AS TipLength
FROM dbo.[03_LIBRARY_06_Help Tips] WITH (NOLOCK)
WHERE LTRIM(RTRIM([Help_tip_form]))  = N'04_REVIEW FORM_04'
  AND LTRIM(RTRIM([Help_tip_topic])) = N'Checklist Questions';

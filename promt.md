SELECT COUNT(*) AS Total FROM dbo.[03_LIBRARY_10_Distribution Parties];  -- 930

SELECT Recipient_role, Recipient_name, Recipient_email
FROM dbo.[03_LIBRARY_10_Distribution Parties]
WHERE Recipient_email IN ('test.alpha@example-test.com',
                          'DAKERS@firsthorizon.com',
                          'JADAMS@firsttennessee.com');
-- TESTUSER (99901) hona chahiye, AKERS nahi, aur JADAMS ki ID '00910'

SELECT TOP 1 * FROM dbo.[05_AUDIT_01_Distribution Parties Uploads]
ORDER BY Uploaded_at_utc DESC;
-- tumhara naam, file name, aur Added 1 / Removed 1 / Changed 0


SELECT Recipient_role, Recipient_name FROM dbo.[03_LIBRARY_10_Distribution Parties];

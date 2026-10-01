SELECT Recipient_role, Recipient_name FROM dbo.[03_LIBRARY_10_Distribution Parties]
WHERE Recipient_name LIKE 'TESTUSER%' OR Recipient_name LIKE 'SINGH, MANIKANT%'
   OR Recipient_email = 'DAKERS@firsthorizon.com';
-- sirf AKERS aana chahiye


DROP TABLE dbo.[DP_backup_test];

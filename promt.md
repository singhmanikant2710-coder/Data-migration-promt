-- Dropdown mein jitne naam dikh rahe hain, utne hi yahan hone chahiye
SELECT COUNT(*) AS TotalParties
FROM dbo.[03_LIBRARY_10_Distribution Parties]
WHERE NULLIF(LTRIM(RTRIM(Recipient_name)), '') IS NOT NULL;

-- Confirm karo ki role ka koi column bacha hi nahi (saare Employee IDs hain)
SELECT TOP 10 Recipient_role, Recipient_name, Recipient_email
FROM dbo.[03_LIBRARY_10_Distribution Parties]
ORDER BY Recipient_name;

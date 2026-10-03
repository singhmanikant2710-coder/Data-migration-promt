SELECT Recipient_name, COUNT(*) AS Cnt
FROM dbo.[03_LIBRARY_10_Distribution Parties]
GROUP BY Recipient_name
HAVING COUNT(*) > 1;

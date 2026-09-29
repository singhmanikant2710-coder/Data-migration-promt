-- Covenant rows hain lekin us month ki tblMain row nahi (yahi fallback trigger karta hai)
SELECT DISTINCT c.strCustomerName, c.strMonthKey
FROM tblMainCovenants c
WHERE NOT EXISTS (SELECT 1 FROM tblMain m
                  WHERE m.strCustomerName = c.strCustomerName
                    AND m.strMonthKey = c.strMonthKey)
ORDER BY c.strCustomerName, c.strMonthKey;

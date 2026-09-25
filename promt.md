SELECT m.strMonthKey, s.slot, s.name AS tblMain_SlotName,
       s.actual AS tblMain_Actual, s.fmt AS tblMain_Formatted,
       c.strCovenantActual AS tblMainCovenants_Actual,
       CASE WHEN ISNULL(s.actual, -1) <> ISNULL(TRY_CONVERT(float, c.strCovenantActual), -1)
            THEN 'MISMATCH' ELSE '' END AS Check_
FROM tblMain m
CROSS APPLY (VALUES
  (1, m.strCovenantName1, m.dblCovenantActual1, m.dblCovenantActual1Formatted),
  (2, m.strCovenantName2, m.dblCovenantActual2, m.dblCovenantActual2Formatted),
  (3, m.strCovenantName3, m.dblCovenantActual3, m.dblCovenantActual3Formatted),
  (4, m.strCovenantName4, m.dblCovenantActual4, m.dblCovenantActual4Formatted),
  (5, m.strCovenantName5, m.dblCovenantActual5, m.dblCovenantActual5Formatted),
  (6, m.strCovenantName6, m.dblCovenantActual6, m.dblCovenantActual6Formatted)
) s(slot, name, actual, fmt)
LEFT JOIN tblMainCovenants c
  ON c.strCustomerName = m.strCustomerName
 AND c.strMonthKey = m.strMonthKey
 AND c.strCovenantName = s.name
WHERE m.strCustomerName LIKE 'ATHENS PAPER%'
  AND m.strMonthKey BETWEEN '202501' AND '202612'
  AND s.name = 'Min Tangible Net Worth'
ORDER BY m.strMonthKey, s.slot;

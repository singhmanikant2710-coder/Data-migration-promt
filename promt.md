SELECT
    m.strMonthKey,
    1 AS slot,
    m.strCovenantName1 AS tblMain_SlotName,
    m.dblCovenantActual1 AS tblMain_Actual,
    m.dblCovenantActual1Formatted AS tblMain_Formatted,
    c.strCovenantActual AS tblMainCovenants_Actual,
    IIf(
        Nz(m.dblCovenantActual1,-1) <>
        IIf(
            Nz(c.strCovenantActual,"")="",
            -1,
            Val(Replace(Replace(c.strCovenantActual,"$",""),",",""))
        ),
        "MISMATCH",
        ""
    ) AS [Check]
FROM tblMain AS m
LEFT JOIN tblMainCovenants AS c
    ON c.strCustomerName = m.strCustomerName
    AND c.strMonthKey = m.strMonthKey
    AND c.strCovenantName = m.strCovenantName1
WHERE Trim(m.strCustomerName) = "ATHENS PAPER COMPANY INC"
AND m.strMonthKey BETWEEN "202501" AND "202612"
AND m.strCovenantName1 = "Min Tangible Net Worth"

UNION ALL

SELECT
    m.strMonthKey,
    2 AS slot,
    m.strCovenantName2 AS tblMain_SlotName,
    m.dblCovenantActual2 AS tblMain_Actual,
    m.dblCovenantActual2Formatted AS tblMain_Formatted,
    c.strCovenantActual AS tblMainCovenants_Actual,
    IIf(
        Nz(m.dblCovenantActual2,-1) <>
        IIf(
            Nz(c.strCovenantActual,"")="",
            -1,
            Val(Replace(Replace(c.strCovenantActual,"$",""),",",""))
        ),
        "MISMATCH",
        ""
    ) AS [Check]
FROM tblMain AS m
LEFT JOIN tblMainCovenants AS c
    ON c.strCustomerName = m.strCustomerName
    AND c.strMonthKey = m.strMonthKey
    AND c.strCovenantName = m.strCovenantName2
WHERE Trim(m.strCustomerName) = "ATHENS PAPER COMPANY INC"
AND m.strMonthKey BETWEEN "202501" AND "202612"
AND m.strCovenantName2 = "Min Tangible Net Worth"

UNION ALL

SELECT
    m.strMonthKey,
    3 AS slot,
    m.strCovenantName3 AS tblMain_SlotName,
    m.dblCovenantActual3 AS tblMain_Actual,
    m.dblCovenantActual3Formatted AS tblMain_Formatted,
    c.strCovenantActual AS tblMainCovenants_Actual,
    IIf(
        Nz(m.dblCovenantActual3,-1) <>
        IIf(
            Nz(c.strCovenantActual,"")="",
            -1,
            Val(Replace(Replace(c.strCovenantActual,"$",""),",",""))
        ),
        "MISMATCH",
        ""
    ) AS [Check]
FROM tblMain AS m
LEFT JOIN tblMainCovenants AS c
    ON c.strCustomerName = m.strCustomerName
    AND c.strMonthKey = m.strMonthKey
    AND c.strCovenantName = m.strCovenantName3
WHERE Trim(m.strCustomerName) = "ATHENS PAPER COMPANY INC"
AND m.strMonthKey BETWEEN "202501" AND "202612"
AND m.strCovenantName3 = "Min Tangible Net Worth"

UNION ALL

SELECT
    m.strMonthKey,
    4 AS slot,
    m.strCovenantName4 AS tblMain_SlotName,
    m.dblCovenantActual4 AS tblMain_Actual,
    m.dblCovenantActual4Formatted AS tblMain_Formatted,
    c.strCovenantActual AS tblMainCovenants_Actual,
    IIf(
        Nz(m.dblCovenantActual4,-1) <>
        IIf(
            Nz(c.strCovenantActual,"")="",
            -1,
            Val(Replace(Replace(c.strCovenantActual,"$",""),",",""))
        ),
        "MISMATCH",
        ""
    ) AS [Check]
FROM tblMain AS m
LEFT JOIN tblMainCovenants AS c
    ON c.strCustomerName = m.strCustomerName
    AND c.strMonthKey = m.strMonthKey
    AND c.strCovenantName = m.strCovenantName4
WHERE Trim(m.strCustomerName) = "ATHENS PAPER COMPANY INC"
AND m.strMonthKey BETWEEN "202501" AND "202612"
AND m.strCovenantName4 = "Min Tangible Net Worth"

UNION ALL

SELECT
    m.strMonthKey,
    5 AS slot,
    m.strCovenantName5 AS tblMain_SlotName,
    m.dblCovenantActual5 AS tblMain_Actual,
    m.dblCovenantActual5Formatted AS tblMain_Formatted,
    c.strCovenantActual AS tblMainCovenants_Actual,
    IIf(
        Nz(m.dblCovenantActual5,-1) <>
        IIf(
            Nz(c.strCovenantActual,"")="",
            -1,
            Val(Replace(Replace(c.strCovenantActual,"$",""),",",""))
        ),
        "MISMATCH",
        ""
    ) AS [Check]
FROM tblMain AS m
LEFT JOIN tblMainCovenants AS c
    ON c.strCustomerName = m.strCustomerName
    AND c.strMonthKey = m.strMonthKey
    AND c.strCovenantName = m.strCovenantName5
WHERE Trim(m.strCustomerName) = "ATHENS PAPER COMPANY INC"
AND m.strMonthKey BETWEEN "202501" AND "202612"
AND m.strCovenantName5 = "Min Tangible Net Worth"

UNION ALL

SELECT
    m.strMonthKey,
    6 AS slot,
    m.strCovenantName6 AS tblMain_SlotName,
    m.dblCovenantActual6 AS tblMain_Actual,
    m.dblCovenantActual6Formatted AS tblMain_Formatted,
    c.strCovenantActual AS tblMainCovenants_Actual,
    IIf(
        Nz(m.dblCovenantActual6,-1) <>
        IIf(
            Nz(c.strCovenantActual,"")="",
            -1,
            Val(Replace(Replace(c.strCovenantActual,"$",""),",",""))
        ),
        "MISMATCH",
        ""
    ) AS [Check]
FROM tblMain AS m
LEFT JOIN tblMainCovenants AS c
    ON c.strCustomerName = m.strCustomerName
    AND c.strMonthKey = m.strMonthKey
    AND c.strCovenantName = m.strCovenantName6
WHERE Trim(m.strCustomerName) = "ATHENS PAPER COMPANY INC"
AND m.strMonthKey BETWEEN "202501" AND "202612"
AND m.strCovenantName6 = "Min Tangible Net Worth"

ORDER BY strMonthKey, slot;

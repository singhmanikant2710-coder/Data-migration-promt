SELECT intFiscalYearMonthStart, strCustomerName
FROM (
    SELECT c.intFiscalYearMonthStart,
           c.strCustomerName,
           ROW_NUMBER() OVER (
               PARTITION BY c.intFiscalYearMonthStart 
               ORDER BY c.strCustomerName
           ) AS rn
    FROM tblCustomer c
    WHERE c.intFiscalYearMonthStart IS NOT NULL
) t
WHERE rn = 1
ORDER BY intFiscalYearMonthStart;


SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/app/blackbook/edit/page.tsx, only the handleAddNewMonth success path. Show unified diff BEFORE applying.

BUG: After adding a new month (e.g. 202608), "New Month" stays 202608 instead of updating to 202609. Cause: the post-add code calls getMaxMonthKey(name), but getMaxMonthKey is client-cached (getOnce), so it returns the STALE cached value within the session. So maxMonthKey isn't updated to the newly-added month, and newMonthKey (= maxMonthKey + 1) stays 202608. Legacy updates New Month instantly on add.

FIX: After a successful add, directly setMaxMonthKey(mk) with the newly-added month key — don't rely on the cached getMaxMonthKey. Since the newMonthKey effect depends on [maxMonthKey], this instantly recomputes New Month to mk+1 (202609), matching legacy.

Find this part in handleAddNewMonth (post-add refresh, after setSelectedMonthKey(mk)):
    setMonthKeys(arr);
    setSelectedMonthKey(mk);
    // Update max month key indicator
    try {
        const mmk = await getMaxMonthKey(name.trim());
        setMaxMonthKey(mmk);
    } catch {}

Change the "Update max month key indicator" block to set maxMonthKey directly to the added month (mk), since mk is now the latest. This avoids the stale client cache:
    setMonthKeys(arr);
    setSelectedMonthKey(mk);
    // The newly-added month is now the latest — set maxMonthKey directly so
    // "New Month" recomputes to mk + 1 instantly (legacy parity). Don't rely on
    // the client-cached getMaxMonthKey, which returns a stale value this session.
    setMaxMonthKey(mk);

Replace the try/getMaxMonthKey/setMaxMonthKey block with the single direct setMaxMonthKey(mk).

STRICT — do NOT change anything else in handleAddNewMonth (the series/rolling24/monthKeys refetch, copyPrev logic, edit-mode, toast, error handling all stay). Only replace the getMaxMonthKey block with setMaxMonthKey(mk).

VERIFY BEFORE SHOWING DIFF:
a) After add, setMaxMonthKey(mk) is called directly (mk = the added month).
b) The getMaxMonthKey(name)/mmk block is removed (replaced by direct set).
c) Everything else in the handler unchanged.

Show the unified diff. Apply nothing until I confirm.

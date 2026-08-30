In frontend/src/app/blackbook/edit/page.tsx, find the latestPoint useMemo. Add ONE console.log right before its return statement:

console.log("[BCAT] selectedMonthKey=", selectedMonthKey, "| src months=", src.map(p => p?.monthKey), "| match=", match?.monthKey);

Apply only this one line.

Before applying, one clarification — quote the dependency array of THIS exact effect (the one containing this default-month block and setSelectedMonthKey). Confirm verbatim it is [name, selectedYear, maxMonthKey] and does NOT include selectedMonthKey. 

Also confirm: is there any OTHER useEffect anywhere in the file that calls setSelectedMonthKey (e.g. a reset effect, or one depending on series/current-year load)? If yes, quote it — because even with this fix, another effect could still overwrite the user's selection. List every setSelectedMonthKey call site in the file and its effect's dependency array.

Just answer this (quote the deps + list all setSelectedMonthKey call sites). Then I'll confirm apply.

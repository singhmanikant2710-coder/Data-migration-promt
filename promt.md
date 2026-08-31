SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/app/blackbook/edit/page.tsx, only the useEffect that computes newMonthKey. Show unified diff BEFORE applying.

BUG: "New Month" is computed from selectedMonthKey (falling back to maxMonthKey), so it changes when the user selects a different month. It should ALWAYS be the latest existing month + 1 (like legacy), independent of the selected month.

VERIFIED: For BHG, "Account Details Loaded Through: 202605" (so maxMonthKey = 202605, the overall latest — correct). New Month currently shows 202605 (because 202604 is selected → selected+1), but should show 202606 (maxMonthKey 202605 + 1). Legacy shows New Month as latest+1, constant.

FIX: Base newMonthKey on maxMonthKey only, and remove selectedMonthKey from the dependency.

Current:
    useEffect(() => {
      const mk = (selectedMonthKey || maxMonthKey || "");
      setNewMonthKey(nextMonthKey(mk));
    }, [selectedMonthKey, maxMonthKey]);

Change to:
    useEffect(() => {
      const mk = (maxMonthKey || "");
      setNewMonthKey(nextMonthKey(mk));
    }, [maxMonthKey]);

Only this change. Do NOT touch nextMonthKey (it correctly handles Dec→Jan rollover), the <Select> for newMonthKey, the Add New Month flow, or anything else.

VERIFY BEFORE SHOWING DIFF:
a) newMonthKey now derives from maxMonthKey only (not selectedMonthKey).
b) Dependency array is [maxMonthKey] only.
c) nextMonthKey and everything else unchanged.

Show the unified diff. Apply nothing until I confirm.

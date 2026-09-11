Apply the diff exactly: change loadMonthKeys' def seed to:
    let def = arr.includes(selectedMonthKey) ? selectedMonthKey : "";
Everything else unchanged. Apply now, then run typecheck/build.

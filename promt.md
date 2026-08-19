Single-file edit: frontend/src/app/load-samples/page.tsx

Fix (#176 Items 2 & 3): Hide "Quarter" and "Closed" columns ONLY when creating a new sample (editingParentId === NEW_ID). Show them normally otherwise (including when searching/viewing existing Open/Closed samples). When hidden, widen the EIC Name, Type, and Target (BU) dropdown fields to use the freed space.

STEP 1 — Add an isCreating flag at the top of the parentColumns useMemo:

const isCreating = editingParentId === NEW_ID;

STEP 2 — Widen the three dropdown cells conditionally (when isCreating, use wider classes; otherwise keep current widths).

For EIC Name cell, change:
BEFORE:
<div onClick={(e) => e.stopPropagation()} onKeyDown={(e) => e.stopPropagation()} className="w-28 min-w-0">
  <SearchableSelect
    options={eicOptions}
    ...
    className="w-32"
    ...
AFTER:
<div onClick={(e) => e.stopPropagation()} onKeyDown={(e) => e.stopPropagation()} className={isCreating ? "w-48 min-w-0" : "w-28 min-w-0"}>
  <SearchableSelect
    options={eicOptions}
    ...
    className={isCreating ? "w-48" : "w-32"}
    ...

For Type <Select>, change:
BEFORE:
<Select
  className="w-28"
  ...
AFTER:
<Select
  className={isCreating ? "w-40" : "w-28"}
  ...

For Target (BU) cell, change:
BEFORE:
<div onClick={(e) => e.stopPropagation()} onKeyDown={(e) => e.stopPropagation()} className="w-28 min-w-0">
  <SearchableSelect
    options={targetOptions}
    ...
    className="w-32"
    ...
AFTER:
<div onClick={(e) => e.stopPropagation()} onKeyDown={(e) => e.stopPropagation()} className={isCreating ? "w-48 min-w-0" : "w-28 min-w-0"}>
  <SearchableSelect
    options={targetOptions}
    ...
    className={isCreating ? "w-48" : "w-32"}
    ...

STEP 3 — At the end of the useMemo, instead of returning the raw array literal, build it as a variable and filter out Quarter/Closed when isCreating, before returning:

Change the useMemo body from:
  return useMemo(() => ([
    { key: "sample_name", ... },
    { key: "sample_start_date", ... },
    { key: "sample_end_date", ... },
    { key: "sample_quarter", ... },
    { key: "eic_Name", ... },
    { key: "sample_type", ... },
    { key: "sample_target", ... },
    { key: "closed", ... }
  ]), [editingParentId, parentDraft, eicOptions, eicLoading, targetOptions, targetLoading]);

To:
  return useMemo(() => {
    const isCreating = editingParentId === NEW_ID;
    const allColumns: Array<Column<Sample>> = [
      { key: "sample_name", ... },
      { key: "sample_start_date", ... },
      { key: "sample_end_date", ... },
      { key: "sample_quarter", ... },
      { key: "eic_Name", ... },
      { key: "sample_type", ... },
      { key: "sample_target", ... },
      { key: "closed", ... }
    ];
    return isCreating
      ? allColumns.filter(c => c.key !== "sample_quarter" && c.key !== "closed")
      : allColumns;
  }, [editingParentId, parentDraft, eicOptions, eicLoading, targetOptions, targetLoading]);

(Keep every column's existing render function exactly as-is — only wrap the array construction in this isCreating check, and only change the width classNames as shown in Step 2.)

CONSTRAINTS:
- Do NOT change any render logic other than the width className additions shown.
- Do NOT change sample_start_date, sample_end_date, or sample_name columns.
- Do NOT change the dependency array.
- When NOT creating (isCreating = false), Quarter and Closed must render EXACTLY as before — no visual change in search/view mode.
- Do NOT touch any other file.
- Show the FULL diff, and show the complete final parentColumns block so I can verify the filter and widths are correctly placed.

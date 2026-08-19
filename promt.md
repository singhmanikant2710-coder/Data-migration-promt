Single-file edit: frontend/src/app/load-samples/page.tsx

GOAL: Make BOTH grids (Select Sample and Load Samples) behave exactly like the Review Queue page — the card fits the screen, and the wide table scrolls horizontally INSIDE the card. Review Queue does this with: overflow-x-auto wrapper + DataTable className="w-full" + NO fixed min-w. Replicate that here, cleanly, without breaking JSX.

=== SELECT SAMPLE GRID ===
Find the Select Sample grid block. Its current structure is:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  <div className="min-w-[1200px]">   (or min-w-[1000px] — whatever it currently is)
    <div className="flex items-center justify-between px-2 py-1 ...">  {/* "Showing X of Y" bar */}
      ...loading/showing text...
    </div>
    <DataTable<Sample>
      columns={parentColumns}
      ...all existing props...
    />
    <Pagination ... />
  </div>
</div>

Rewrite it to REMOVE the min-w wrapper and add w-full to the DataTable, keeping the "Showing" bar and Pagination inside, exactly like this:
<div className="mt-3 border border-slate-200 rounded-lg shadow-sm overflow-x-auto w-full">
  <div className="flex items-center justify-between px-2 py-1 bg-[color:var(--brand-surface)] text-xs text-slate-600 gap-2 mb-1">
    {parentLoading ? (
      <ThemedLoading label="Loading..." size="sm" emphasis="subtle" />
    ) : parentSaving ? (
      <ThemedLoading label="Saving..." size="sm" emphasis="subtle" />
    ) : (
      `Showing ${parentPagedRows.length} of ${parentFilteredRows.length}`
    )}
  </div>
  <DataTable<Sample>
    className="w-full"
    columns={parentColumns}
    rows={parentPagedRows}
    sortBy={parentSortBy}
    sortDir={parentSortDir}
    onSort={onParentSort}
    selectedId={selectedParentId ?? undefined}
    editingId={editingParentId ?? undefined}
    getRowId={(r) => String(r.sample_id)}
    onSelect={(id) => setSelectedParentId(id)}
    onOpen={(row) => { setSelectedParentId(String(row.sample_id)); }}
    rowActions={parentRowActions}
    tableLabel="Samples"
    selectedRowClassName="bg-[color:var(--brand-primary)]/10 outline outline-2 outline-[color:var(--brand-primary)]/50"
  />
  <Pagination
    currentPage={parentPage}
    pageSize={parentPageSize}
    total={parentSortedRows.length}
    onFirst={() => setParentPage(1)}
    onPrev={() => setParentPage(p => Math.max(1, p - 1))}
    onNext={() => setParentPage(p => Math.min(Math.max(1, Math.ceil(parentSortedRows.length / parentPageSize)), p + 1))}
    onLast={() => setParentPage(Math.max(1, Math.ceil(parentSortedRows.length / parentPageSize)))}
    className="px-2 py-1"
  />
</div>

IMPORTANT: Keep ALL the existing DataTable props exactly as they currently are — I've listed the common ones above, but if the real file has additional props, KEEP them. Only two structural changes: (1) remove the <div className="min-w-[...]"> wrapper (unwrap its children up one level), (2) add className="w-full" to the DataTable. Do NOT drop any prop, the Showing bar, or the Pagination.

=== LOAD SAMPLES (CHILD) GRID ===
Apply the SAME transformation to the child grid: remove its <div className="min-w-[1000px]"> wrapper, keep the "Showing" bar and its action buttons and Pagination inside the outer overflow-x-auto div, and add w-full to the child DataTable's className (it currently has className="relative z-10 mt-2" — change to className="relative z-10 mt-2 w-full"). Keep ALL existing child DataTable props and the Pagination exactly as they are.

=== CONSTRAINTS ===
- Do NOT remove or reorder any DataTable prop, the Showing/loading bar, the action buttons, or Pagination — only remove the min-w wrapper div and add w-full to each DataTable.
- Do NOT change parentColumns, childColumns, dropdown widths (keep isCreating w-44), or any handler/state/logic.
- Do NOT touch AppShell or shared components.
- Ensure the JSX is valid (matching opening/closing tags) after removing the wrapper div — this is critical, the last attempt broke the structure.
- Show the FULL final JSX of BOTH grid blocks (not just a diff) so I can verify the structure is clean and complete.

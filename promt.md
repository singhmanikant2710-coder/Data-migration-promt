READ-ONLY. Confirm the index mismatch in CovenantsSection. Quote with paths.

HYPOTHESIS: CovenantsSection renders a SORTED/reordered covenant list (by covOrderMap/order), but the input onChange calls onRowEdit(idx, {threshold/description}) with the SORTED index. In page.tsx, onCovenantRowEdit reads covenants[index] from the ORIGINAL (unsorted) array. So the index points to the wrong row → threshold/description are captured against the wrong row (or a blank row with no originalName), so they never reach covEdits for the correct covenant → payload falls back to { order }.

In frontend/src/app/customer/edit/components/CustomerEditParts.tsx (CovenantsSection):
1) Quote how the rows are rendered — is the list sorted/reordered before .map()? Quote the array being mapped (e.g. rows.sort(...) or a derived sortedRows). What index is passed to onRowEdit — the map index of the sorted array, or the original row index?
2) Quote the threshold and description onChange → onRowEdit(idx OR index, {...}). Is idx the sorted-map index?
3) In frontend/src/app/customer/edit/page.tsx onCovenantRowEdit(index, patch): quote how it uses index — covenants[index]. Is 'covenants' the same order as what CovenantsSection renders, or unsorted/different?
4) Confirm: does the index passed from the sorted UI match covenants[index] in page.tsx? If the UI is sorted and page uses unsorted covenants[index], that's the mismatch.

OUTPUT:
- A) The array CovenantsSection maps (sorted?) + index passed to onRowEdit, quoted.
- B) How page.tsx onCovenantRowEdit resolves the row from index (covenants[index]), quoted.
- C) Do the indices align, or is there a sorted-vs-unsorted mismatch? State clearly.
- D) Exact fix: pass a STABLE row identifier (covenant name or id) to onRowEdit instead of a positional index, OR map the sorted index back to the covenants array index. Name the location.
- No fix. Findings only.

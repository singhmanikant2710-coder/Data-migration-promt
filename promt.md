Frontend only. Single file: frontend/src/app/review-status/page.tsx
Do NOT modify the backend or any other file. Do NOT switch to the DataTable component — keep the existing plain <table>. Do not plan. Just apply.

UAT #165: Add ascending/descending column sorting to the Review Status table, matching Review Queue's behaviour (click a header to sort; click again to flip direction).

1) Add sort state near the other state (e.g. next to selectedBucket / currentPage):
     const [sortBy, setSortBy] = React.useState<string>("borrower");
     const [sortDir, setSortDir] = React.useState<"asc" | "desc">("asc");

2) Add a handleSort function (same pattern as Review Queue):
     const handleSort = (key: string) => {
       if (key === sortBy) {
         setSortDir((d) => (d === "asc" ? "desc" : "asc"));
       } else {
         setSortBy(key);
         setSortDir("asc");
       }
     };

3) Add a sortedRows useMemo that sorts filteredRows (do NOT change filteredRows itself). Use these field keys and normalisation:
   - "reviewId" -> Number
   - "exposure" -> numeric (strip any currency/commas then Number; fall back to 0)
   - "bankPd", "casPd" -> Number
   - "completed" -> new Date(String(val)).getTime()
   - "borrower", "reviewer", "status", "manager" -> String(val).toLowerCase()
   Sort ascending/descending per sortDir. Signature:
     const sortedRows = React.useMemo(() => {
       const arr = [...filteredRows];
       const key = sortBy;
       const norm = (val: any) => { /* the switch above */ };
       arr.sort((a, b) => {
         const na = norm((a as any)[key]);
         const nb = norm((b as any)[key]);
         if (na < nb) return sortDir === "asc" ? -1 : 1;
         if (na > nb) return sortDir === "asc" ? 1 : -1;
         return 0;
       });
       return arr;
     }, [filteredRows, sortBy, sortDir]);

4) Change pagedRows to slice from sortedRows instead of filteredRows (so sorting applies before pagination). Update its dependency array accordingly.

5) Make each column header clickable. For each <th>, add an onClick calling handleSort with the matching field key, a cursor-pointer class, and a small asc/desc indicator (▲/▼) shown only on the active sort column. Header-to-key mapping:
   - "Review Id" -> "reviewId"
   - "Borrower Name / Linesheet" -> "borrower"
   - "Reviewer" -> "reviewer"
   - "Exposure" -> "exposure"
   - "Bank PD" -> "bankPd"
   - "CAS PD" -> "casPd"
   - "Review Status" -> "status"
   - the {completedColLabel} column -> "completed"
   - "Approver" -> "manager"

Do NOT change the row rendering (the DocIcon links, the reviewId/borrower links, the ${r.reviewId}-${r.ecif} row key), the tile counts, the bucket switch, byText, or the empty-state row. Only add sorting.

Run read-only TypeScript diagnostics on this file only.

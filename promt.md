SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/app/customer/edit/page.tsx, only the onOrderChange prop passed to CovenantsSection. Show unified diff BEFORE applying.

GOAL: Add legacy-style validation — when a user sets a covenant Order to a value that is already used (non-zero) by ANOTHER covenant, show a warning and block the change, matching legacy: "Please only select one Order value for BlackBook display. 'X' has been selected more than once. To update, first change existing value to '0'."

Current:
    onOrderChange={(nameKey, v) => setCovOrderMap(prev => ({ ...prev, [nameKey]: v }))}

Change to (validate uniqueness for non-zero values; block + warn on duplicate):
    onOrderChange={(nameKey, v) => {
      // Order 0 = hidden/unranked; allow duplicates of 0 freely.
      if (v !== 0) {
        // Check if another covenant already uses this non-zero order.
        const conflict = Object.entries(covOrderMap).some(
          ([n, ord]) => n !== nameKey && Number(ord) === v
        );
        if (conflict) {
          try {
            toast.showWarning(
              `Please only select one Order value for BlackBook display. '${v}' has been selected more than once. To update, first change the existing value to '0'.`
            );
          } catch {}
          return; // block the update
        }
      }
      setCovOrderMap(prev => ({ ...prev, [nameKey]: v }));
    }}

Confirm toast is available in this scope (const toast = useToast() or similar). If it's toast.showWarning that doesn't exist, use toast.showError or the available warning method — quote which toast methods exist.

STRICT: only change this one onOrderChange handler. Nothing else.

VERIFY BEFORE SHOWING DIFF:
a) v===0 allows duplicates (multiple covenants can be 0/hidden).
b) v!==0 checks covOrderMap for another covenant with same order; if found, warn + return (block).
c) toast method used actually exists (quote available toast methods).

Show the unified diff. Apply nothing until I confirm.

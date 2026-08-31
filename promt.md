READ-ONLY. One pass. Find the Covenants save flow and why the 'customer' query parameter is empty. Quote with file paths.

ERROR (from UI): Save failed - {"title":"Invalid customer name","status":400,"detail":"Provide a non-empty 'customer' query parameter."} on the Customer View/Edit → Covenants section. Also "Partial save: Some covenant changes could not be saved."

1) Find the Covenants component/section on the Customer View/Edit screen. Locate the SAVE handler that calls the covenants save API. Quote the API call (URL + method + query/body), especially how it passes 'customer'.
2) Where does the customer name come from in this component? Quote it — is it from useSearchParams (?name=), a prop, route param, or local state? Is it the same source the blackbook edit page uses (which works)?
3) At save time, is that customer value populated or empty/undefined? Trace whether it's read correctly. Quote the exact object passed as the query, e.g. { customer: <something> }.
4) If covenants are saved per-row (loop), does EACH row's save call include the customer param, or does one path omit it? (The "partial save" suggests some succeed, some fail.)
5) Compare with a working save on the same screen (if any other section saves successfully) — how does IT pass customer? Quote the difference.

OUTPUT:
- A) Covenants save API call + how 'customer' is passed, quoted.
- B) Source of customer name in the component, quoted (and whether it matches the working blackbook page's source).
- C) Why 'customer' is empty at save time.
- D) If per-row: which path omits customer (explaining "partial save").
- E) Exact fix location + what the customer value should be.
- One pass. Findings only. No fix.

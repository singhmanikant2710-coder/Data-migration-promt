SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/app/customer/edit/page.tsx, only the onSaveCovenantOrder function's API call. Show unified diff BEFORE applying.

BUG: The bulk covenant order save fails with 400 "Provide a non-empty 'customer' query parameter." because the POST to /api/v1/covenants/order does NOT include the 'customer' query parameter. It only puts CustomerName in the JSON body, but the server requires ?customer= in the query. This causes the "Partial save" warning (per-row saves succeed, the order save fails).

FIX: Add the 'customer' query parameter (= qpName, already validated earlier in this function) to the POST call.

Current call:
    await apiClient.post<CovenantOrderBulkUpdate, Covenant[]>("/api/v1/covenants/order", payload);

Change to include the customer query param. Match the exact query-passing pattern apiClient uses elsewhere in this file. Two options depending on apiClient's signature:

Option A — if apiClient.post supports an options/query arg (like put does with { query: {...} }):
    await apiClient.post<CovenantOrderBulkUpdate, Covenant[]>("/api/v1/covenants/order", payload, { query: { customer: qpName } });

Option B — if it doesn't, append to the URL:
    await apiClient.post<CovenantOrderBulkUpdate, Covenant[]>(`/api/v1/covenants/order?customer=${encodeURIComponent(qpName)}`, payload);

First CHECK the apiClient.post signature (look at how other post/put calls in this file pass query params — e.g. the read uses ?customer=... in the URL, and updateCovenantByComposite uses { query: {...} } with put). Use whichever pattern apiClient actually supports. Quote the apiClient.post signature so we pick the right option.

Also keep CustomerName in the body (don't remove it) — the server may use both; we're only ADDING the query param.

VERIFY BEFORE SHOWING DIFF:
a) The POST to /api/v1/covenants/order now includes customer=qpName in the query.
b) qpName is the validated customer name already used earlier in onSaveCovenantOrder.
c) The body payload is unchanged (CustomerName stays).
d) Used the correct apiClient pattern (quote its signature).

Show the unified diff. Apply nothing until I confirm.

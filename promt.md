SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/lib/covenants.ts, only updateCovenantByComposite. Show unified diff BEFORE applying.

BUG CONFIRMED VIA NETWORK: The failing call is PUT /api/v1/covenants/by-name → 400 "Provide a non-empty 'customer' query parameter." The function updateCovenantByComposite passes { query: { customer, monthKey, covenantName } } to put(). BUT apiClient merges opts.query into the request BODY for non-GET requests (it does NOT append to the URL). So the server never receives ?customer=/?monthKey=/?covenantName= in the query string, and returns 400.

FIX: Append these three params to the URL directly (like we did for the order save), instead of relying on { query } which goes into the body.

Current:
export async function updateCovenantByComposite(customer: string, monthKey: string, covenantName: string, update: CovenantUpdateReq): Promise<void> {
  const cust = String(customer || "").trim();
  const mk = String(monthKey || "").trim();
  const cov = String(covenantName || "").trim();
  if (!cust || !mk || !cov) return;
  await put<CovenantUpdateReq, void>("/api/v1/covenants/by-name", update, { query: { customer: cust, monthKey: mk, covenantName: cov } });
}

Change the put call to append the params to the URL:
  const qs = `customer=${encodeURIComponent(cust)}&monthKey=${encodeURIComponent(mk)}&covenantName=${encodeURIComponent(cov)}`;
  await put<CovenantUpdateReq, void>(`/api/v1/covenants/by-name?${qs}`, update);

Keep the trims and the guard (if !cust || !mk || !cov return) unchanged. Only change how the params are passed — from { query } (which apiClient puts in the body) to URL query string.

VERIFY BEFORE SHOWING DIFF:
a) customer, monthKey, covenantName are now in the URL query string (encoded), not in { query }.
b) The guard and trims are unchanged.
c) The body (update) is still passed as the body.

Show the unified diff. Apply nothing until I confirm.

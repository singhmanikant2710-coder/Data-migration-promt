const qs = `customer=${encodeURIComponent(cust)}&monthKey=${encodeURIComponent(mk)}&covenantName=${encodeURIComponent(cov)}`;
await put<CovenantUpdateReq, void>(`/api/v1/covenants/by-name?${qs}`, update);

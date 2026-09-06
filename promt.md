Bug 213 — Maintenance > Policy Exceptions library sorts by Category then Code, but should sort by Code alone. READ-ONLY, no edits. One pass, answer, STOP.

Screen: Maintenance > Policy Exceptions library (the maintenance/library list of policy exceptions).

Find where this list is sorted:
1. Frontend Policy Exceptions maintenance/library page — how does it load and order its rows? Is there a client-side sort? What keys does it sort by (Category then Code)? Paste that code + file/line.
2. Backend — find the endpoint/query that returns the Policy Exceptions library list. Paste the exact ORDER BY. Is it ordering by Category then Code (e.g. ORDER BY Category, Code), or something else?
3. Identify the SINGLE place where the sort order is decided (frontend sort OR backend ORDER BY) that needs to change to sort by Code ALONE.

Report file paths + line numbers + the current sort logic. Do NOT fix yet.

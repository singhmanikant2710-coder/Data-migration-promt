Frontend only. Single file: frontend/src/app/review-status/page.tsx
Do NOT modify the backend. Do not plan. Just apply.

UAT #163: The Review Status grid drops reviews the backend correctly returns (e.g. review 20120), making its counts differ from Review Queue (which is correct). Root cause: the client-side byDate filter in the filteredRows useMemo removes rows whose milestone date falls outside the sample's auto-bound window. Review Queue has no such per-row filter, and the backend already returns the correct rows per bucket.

Confirmed safe: byDate, parseMDY and parseInput are used ONLY inside filteredRows. startDate/endDate are also used elsewhere (state, API load, date inputs) and must be kept.

In frontend/src/app/review-status/page.tsx, inside the filteredRows useMemo ONLY:
1) Delete the byDate function.
2) Delete the parseMDY function.
3) Delete the parseInput function.
4) Change the final return to filter by the text filter only:
     return rows.filter((r) => byText(r));
5) Remove startDate and endDate from THIS useMemo's dependency array only. Do NOT remove the startDate/endDate state declarations or any of their other usages (date inputs, API load).

Do NOT change the selectedBucket switch, byText, tile counts, pagination, the date input fields, or the API load. 

Run read-only TypeScript diagnostics on this file only.

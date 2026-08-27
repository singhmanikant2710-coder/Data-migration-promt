READ-ONLY DIAGNOSTIC. No edits. Read the specified files, quote with file paths, findings only. Do NOT loop — read each file once.

GOAL: Find why "Cash Collections %" shows 0 (and DB perCashCollections=5) when it should be 50% (500000/1000000 prior month). Need to know whether the UI reads the BACKEND-computed tblMain.perCashCollections OR computes it in the FRONTEND, and where the wrong value comes from.

DATA (tblMain, 202607, AMERICAN CREDIT ACCEPTANCE):
- curCashCollections=500000, curPrincipalNR=100000 (current), curPrincipalNRPriorMonth=1000000 (prior)
- selection strPrincipalOrGrossCalculationSelectionCashCollection="Principal N/R"
- perCashCollections (stored in tblMain) = 5  (=500000/100000, using CURRENT month — wrong; should be 0.5 using prior)
- Correct = 500000/1000000 = 0.5 = 50%

Read and quote from BOTH sides:

=== BACKEND: backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs ===
B1) Search "perCashCollections". Quote EVERY place m.[perCashCollections] is SET (sets.Add / UPDATE / MERGE). State how many distinct places. If more than one, quote each — one may use curPrincipalNR (current) and overwrite the fixed one.
B2) Confirm the fixed block now uses curPrincipalNRPriorMonth with LIKE 'PRINCIPAL%'. Quote it as it currently is.
B3) Does the read path (GetCurrentYearSeriesAsync) SELECT perCashCollections from tblMain into the row Values? Quote where perCashCollections is read into the series (the SELECT column list or mapping).

=== FRONTEND: which source does the UI "Cash Collections %" use? ===
F1) In frontend/src/blackbook/mappings/tblMainCalcs.ts: quote perCashCollections (the frontend calc). It uses curPrincipalNRPriorMonth (we saw this — it's correct).
F2) In frontend/src/blackbook/components/monthSummaryRegistry.ts (or monthSummaryProfiles / mapIndirectAuto): find the "Cash Collections %" tile/column render. Does it call the frontend perCashCollections calc, OR does it read a precomputed value from row.values (e.g. row.values.perCashCollections coming from backend tblMain)? Quote the exact render/resolution.
F3) In frontend/src/app/blackbook/edit/page.tsx: in seriesWithEdits, the tblMainCalcs loop runs client-side calcs and writes them onto merged (for(const [key,fn] of Object.entries(tblMainCalcs)) { merged[key]=fn(inputs) }). Does this overwrite perCashCollections with the frontend calc? If so, is curPrincipalNRPriorMonth present in the row inputs (merged), or missing (so frontend calc also fails)? Quote whether curPrincipalNRPriorMonth is hydrated/merged onto the row in seriesWithEdits.

OUTPUT:
- A) BACKEND: how many places set perCashCollections; does any use curPrincipalNR (current)? Quoted.
- B) Does the series read path expose tblMain.perCashCollections to the UI? Quoted.
- C) FRONTEND: does the "Cash Collections %" render use the frontend calc or the backend value? Quoted.
- D) Is curPrincipalNRPriorMonth actually present on the row the frontend calc sees? yes/no + where it's set (or missing). Quoted.
- E) State plainly the ONE reason it's wrong: [backend second path uses current month] OR [frontend reads backend's wrong value] OR [frontend calc runs but curPrincipalNRPriorMonth missing from row so it falls to 0/wrong] OR [selection not matching].
- No fix yet. Findings only.

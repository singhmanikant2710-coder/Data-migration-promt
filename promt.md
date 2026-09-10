READ-ONLY. Find why some derived fields recompute instantly on edit (YTD Revenue) but others don't (YTD PBT, Cash Collections %, YTD Net C/O). Quote with paths.

OBSERVED on edit (1ST FRANKLIN):
- WORKS instantly: YTD Revenue updates when Month Revenue is entered.
- DOES NOT work instantly: YTD PBT (on Month PBT), Cash Collections % (on Cash Collections, using Principal NR/Gross NR), YTD Net C/O (on Net C/O).

In frontend/src/app/blackbook/edit/page.tsx and mappings/tblMainCalcs.ts (or expr/tblMainCalcs.ts):
1) Find seriesWithEdits / latestPointComputed — the loop that runs tblMainCalcs on edited values: for (const [key, fn] of Object.entries(tblMainCalcs)) { merged[key] = fn(inputs); }. Quote it. This is what makes edits recompute live.
2) In tblMainCalcs, is there a calc for YTD Revenue (curRevenueOrSalesYTD)? Quote it — this one works. 
3) Is there a calc for YTD PBT (curProfitBeforeTaxesYTD), YTD Net C/O (curNetChargeOffYTD)? Quote them, or confirm they're MISSING from tblMainCalcs (which would explain why they don't recompute live — they'd only come from the backend on save).
4) For Cash Collections %, the calc exists (perCashCollections) but needs Principal NR/Gross NR PRIOR month. Are those prior-month inputs present in the edited row (merged)? If missing, the calc returns 0 → appears "not calculating". Quote whether prior-month fields are hydrated.
5) Compare: YTD Revenue calc vs YTD PBT calc — why does one exist/work and the other not?

OUTPUT:
- A) The tblMainCalcs live-recompute loop, quoted.
- B) Which YTD/derived calcs EXIST in tblMainCalcs (YTD Revenue yes; YTD PBT? YTD Net C/O? Cash Coll %?), quoted or "missing".
- C) For each non-working field: is it (i) missing from tblMainCalcs entirely, (ii) present but missing input data (prior-month), or (iii) computed only on backend save?
- D) Exact fix: add the missing YTD/derived calcs to tblMainCalcs (or hydrate missing inputs) so they recompute live like YTD Revenue.
- No fix. Findings only.

READ-ONLY. Confirm the exact selection field name mismatch. Read once, quote, stop. No loop.

The frontend perCashCollections reads: i.strPrincipalOrGrossNRCalculationSelectionCashCollection  (note: "GrossNR")

But the DB column (confirmed via SQL) is: strPrincipalOrGrossCalculationSelectionCashCollection  (note: "Gross", no "NR")

Verify:
1) In frontend/src/blackbook/mappings/tblMainCalcs.ts (or the TblMainInputs type / mapping): search for "strPrincipalOrGross" and quote ALL variants used. Is the field the perCashCollections calc reads spelled with "GrossNR" or "Gross"? Quote the exact string.
2) In the backend read path / series mapping: under what key is strPrincipalOrGrossCalculationSelectionCashCollection placed onto the row Values? Quote it. Does it match what the frontend calc reads?
3) Also confirm the same for the NetChargeOff and 60DPD selection fields (strPrincipalOrGrossCalculationSelectionNetChargeOff, ...per60DPD) — do the frontend calcs read them with "Gross" or "GrossNR"? (60+ DPD % works = 50%, so maybe that one is spelled correctly — compare.)

OUTPUT:
- A) Exact field name the frontend perCashCollections reads (GrossNR vs Gross), quoted.
- B) Exact key the backend puts the selection under, quoted.
- C) Do they match? If not, that's the bug — frontend reads a field name that doesn't exist on the row, so selection is undefined, so it falls to the Gross(0) branch → 0.
- D) How does the working 60+ DPD calc spell its selection field? (to confirm the pattern).
- No fix yet. Findings only.

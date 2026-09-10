SINGLE-FILE, BOUNDED EDIT. Only frontend/src/app/blackbook/edit/page.tsx, only canonicalFromLabel (~L2435-2448). Show unified diff BEFORE applying.

BUG: The Cash & Charge-offs middle-panel tiles ("Net C/O $", "Cash Collections $", "60+ DPD $") are generic tiles created WITHOUT aliases. So writeKey resolves to "" and setEdits is never called — edits in this panel are silently discarded (the typed text stays visible only because the input is uncontrolled). "CPLTD (prior period)" works only because canonicalFromLabel has an explicit case for it.

FIX (E1): Add the missing label cases to canonicalFromLabel, alongside the existing "cpltd (prior period)" case, so these tiles resolve to their real column key:

Find the existing case (around L2446):
    if (s === "cpltd (prior period)") return "curCPLTD";   // (or whatever the exact existing case is — quote it)

Add these three cases right next to it:
    if (s === "net c/o $") return "curNetChargeOff";
    if (s === "cash collections $") return "curCashCollections";
    if (s === "60+ dpd $") return "cur60DPD";

Match the exact style/format of the existing cpltd case (same lowercase comparison variable `s`, same return pattern). 

CONFIRM the canonical column names are correct:
- "Net C/O $" → curNetChargeOff (matches netChargeOffMonthlyAliases / the row column)
- "Cash Collections $" → curCashCollections
- "60+ DPD $" → cur60DPD
Quote the existing cpltd case and the surrounding canonicalFromLabel function so we match the pattern exactly and confirm these column names exist as real tblMain columns.

VERIFY BEFORE SHOWING DIFF:
a) Three new cases added, matching the existing cpltd case style.
b) Canonical names: curNetChargeOff, curCashCollections, cur60DPD (confirm these are the real column keys the Top Strip also writes to — e.g. Top Strip Net C/O writes curNetChargeOff).
c) Only canonicalFromLabel changed; nothing else.

Show the unified diff. Apply nothing until I confirm.

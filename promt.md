Apply the diff exactly as shown. Add the three cases to canonicalFromLabel in frontend/src/app/blackbook/edit/page.tsx:
    if (s === "net c/o $") return "curNetChargeOff";
    if (s === "cash collections $") return "curCashCollections";
    if (s === "60+ dpd $") return "cur60DPD";
placed after the "cpltd (prior period)" case, matching the existing bare-if style. Nothing else changes. Apply now, then run typecheck/build and report any errors.

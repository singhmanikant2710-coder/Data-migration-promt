READ-ONLY. Read once, quote, stop. No loop. Find how "Cash Collections %" is formatted for display.

FACT: Backend perCashCollections = 6.174 (ratio, meaning 617.4%). Legacy stores it as 617.40 (already ×100) and displays 617.40%. New app displays 6.17% — WRONG (should be 617.4%). But earlier, perCashCollections=0.5 displayed correctly as 50%. So the percent formatting is inconsistent: 0.5→50% (×100) but 6.174→6.17% (no ×100).

Find in frontend:
1) In frontend/src/blackbook/mappings/util.ts (or wherever formatPercent is): quote the formatPercent function. Does it multiply by 100? Does it have any conditional (e.g. if value <= 1 multiply by 100, else treat as already-percent)? That conditional would explain 0.5→50% but 6.174→6.17%.
2) In monthSummaryRegistry.ts / the "Cash Collections %" render: quote how it formats perCashCollections. Does it call formatPercent(value) or formatPercent(value*100) or format the raw value?
3) Compare with a percent field that works correctly across ranges (e.g. 60+ DPD % which showed 50% and also ~0.99% correctly) — how does it format? Same or different function?

OUTPUT:
- A) formatPercent definition, quoted — does it have a value<=1 conditional?
- B) How "Cash Collections %" render formats the value, quoted.
- C) State plainly why 0.5→50% but 6.174→6.17%: likely formatPercent multiplies by 100 only when value <= 1, treating >1 as already-percent. So 6.174 is shown as 6.17% instead of 617.4%.
- D) Exact fix location.
- No fix yet. Findings only.

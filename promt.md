Bug 202 — PDF icon + tooltip inconsistency across Review Queue, Review Progress, Review History. READ-ONLY, no edits. One pass, answer, STOP.

All three screens show a document icon that opens the CAS Linesheet PDF, but the icon and the tooltip/modal message differ. Goal: (a) all three use the SAME icon — specifically Review Progress should switch to the icon used by Review Queue & Review History; (b) all three tooltips read exactly "Open CAS Linesheet PDF".

Find and report:
1. The Review Queue screen — the PDF/document icon button that opens the CAS Linesheet. Which icon component/name is used, and what tooltip/title/aria-label/modal text does it show? File + line.
2. The Review History screen — same: icon used + tooltip text. File + line. (Confirm it matches Review Queue.)
3. The Review Progress screen — same: which icon it currently uses (the different one) + its tooltip text. File + line.
4. Confirm exactly: (a) which icon Queue & History share that Progress must switch TO, and (b) each screen's current tooltip string, so we can standardize all three to "Open CAS Linesheet PDF".
5. Are these three using a shared component for the PDF icon button, or does each screen define its own? If shared, why do they differ (a prop)?

Report file paths + line numbers + the exact icon names and tooltip strings for all three. Do NOT fix yet.

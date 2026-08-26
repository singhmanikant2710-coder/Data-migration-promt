READ-ONLY. No edits, no terminal. Read frontend/src/blackbook/pdf/BlackBookPdf.tsx ONCE. Quote current state, stop. No loop.

GOAL: Fix defect #26 — the rolling-summary PDF must show ALL available months (legacy shows up to 24 across two sections: current-year block on top, prior-year block below; this customer has 19 months). Currently only ~6 per section render.

Quote verbatim (current state):

1) The block defining MONTHLY_FIRST_ROWS, MONTHLY_CONT_ROWS, monthlyRowsOnFirstPage, monthlyChunks, and seriesYearOnly (~20 lines context).

2) The historyYearOnly / historyYear2Only definitions and where they are sliced (I recall historyYearOnly.slice(0,6) style caps) — quote them and any .slice caps.

3) The disabled rolling24 block: quote the `if (false && ...)` section and R24_FIRST_ROWS / r24Chunks / splitForPages usage.

4) The JSX where monthlyChunks and history chunks are rendered into the PDF — quote how each chunk maps to a page/section, and confirm the existing splitForPages helper is available for pagination.

5) The section heading <Text> above the current-year grid and above the prior-year/history grid (so I know how legacy's "two 12-month sections" map to this file).

OUTPUT:
- Quoted blocks 1-5.
- State plainly: to show all 19 (up to 24) months as legacy does, which exact cap(s) must change — the MONTHLY_FIRST_ROWS=6 single-chunk slice, the history slice(0,6) caps, and/or re-enabling the rolling24 block? List each cap with its current value.
- Confirm whether splitForPages already handles multi-page pagination so removing the 6-cap won't overflow.
- No fix yet. Findings only.

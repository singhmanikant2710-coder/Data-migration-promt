READ-ONLY. Diagnostics only. Do NOT change anything.

The rolling-24-months data reaches the frontend fully (backend returns TOP 24), but the PDF shows only 6 months. The cap is in the frontend column-builder or PDF component. Pinpoint the exact slice/cap.

Show me ONLY (no edits):

1. buildMonthSummaryColumns implementation — find it (likely frontend/src/blackbook/components/monthSummaryRegistry.ts or index.ts, or wherever it's defined). Show the full function. Look specifically for: any .slice(0, 6), .slice(-6), Take(6), a MAX constant, or a loop bounded to 6 that limits how many month columns are built from the series.

2. BlackBookPdf.tsx (frontend/src/blackbook/pdf/BlackBookPdf.tsx) — show how it renders columnsR24 / rolling24. Look for any slicing to 6, a fixed array of 6, or a per-page column limit that caps months. Show the rolling24 / columnsR24 rendering block.

3. Any constant like MAX_MONTH_COLUMNS, MONTHS = 6, WINDOW = 6, or similar in the blackbook frontend (search frontend/src/blackbook for "6", ".slice", "MAX", "MONTHS", "WINDOW").

4. Confirm: at the point columnsR24 is built (page.tsx:410-432), is rolling24 already full (24) or already truncated before buildMonthSummaryColumns is called? Show what rolling24 contains right before that call (any slicing between fetch and column-building?).

Read once. Findings only. No edits. I need the EXACT line where months get capped to 6.

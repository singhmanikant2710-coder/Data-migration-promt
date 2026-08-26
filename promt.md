SINGLE-FILE, BOUNDED EDIT. Only edit frontend/src/blackbook/pdf/BlackBookPdf.tsx. Show unified diff BEFORE applying. Do not run build.

CONTEXT: Legacy rolling-24 report (rpt002...Rolling24Months) shows 24 months in TWO sections of 12 each: the LATEST 12 months on top, and the OLDER 12 months below — NOT one continuous list, and NOT fiscal-year split. Our current code renders rolling24 as one single chunk. We need to split rolling24 into two 12-month halves: top = latest 12, bottom = older 12, matching legacy.

Currently monthlyChunks is:
  const monthlyChunks = (Array.isArray(rolling24) && rolling24.length > 0)
    ? [[...rolling24].sort((a, b) => normMonthKeyInt((a as any)?.monthKey) - normMonthKeyInt((b as any)?.monthKey))]
    : [];

CHANGE the approach:

1) Build a sorted-ascending rolling24 array, then split into two halves — top section = the LATEST 12 (most recent), bottom section = the OLDER 12. Replace the monthlyChunks definition with two arrays:

   const r24Sorted = (Array.isArray(rolling24) && rolling24.length > 0)
     ? [...rolling24].sort((a, b) => normMonthKeyInt((a as any)?.monthKey) - normMonthKeyInt((b as any)?.monthKey))
     : [];
   // Legacy layout: latest 12 on top, older 12 below
   const r24Top = r24Sorted.slice(-12);                          // latest 12
   const r24Bottom = r24Sorted.slice(0, Math.max(0, r24Sorted.length - 12)); // older 12 (up to 12)
   const monthlyChunks = r24Top.length > 0 ? [r24Top] : [];

   (monthlyChunks stays as the TOP section = latest 12, so the existing top-grid render shows the latest 12.)

2) For the BOTTOM section, RE-ENABLE the prior-year history grid slot but feed it r24Bottom instead of historyYearOnly. Find the FY-1 history block we gated earlier:
   {false && historyYearOnly.length > 0 && (
       ... renders historyYearOnly.slice(0, 12) with colsHist ...
   )}
   Change it to render r24Bottom instead:
   - condition: {r24Bottom.length > 0 && (
   - and replace the mapped array historyYearOnly.slice(0, 12) with r24Bottom
   - keep the same table styling/colsHist/renderRow("history"), and the heading — but change heading text to "Historical Summary" (or keep the existing sectionTitle; if it references {prevYear}, change to a static "Historical Summary" since this is now the older-12 block, not a fiscal year).

3) Leave the FY-2 (historyYear2Only) block gated with false && (we don't need it).

DO NOT:
- Do NOT touch the disabled multi-page r24 block (if (false && r24Chunks...)).
- Do NOT change styles, panels, Page size.
- Do NOT change backend.

VERIFY BEFORE SHOWING DIFF (report; don't force):
a) Confirm r24Top = latest 12 months (slice(-12)) and r24Bottom = the older months (slice(0, length-12)), both derived from the ascending-sorted rolling24.
b) Confirm the top grid renders r24Top (via monthlyChunks[0]) and the bottom grid renders r24Bottom.
c) Confirm normMonthKeyInt is available.
d) Confirm the bottom section's heading no longer depends on {prevYear} (since it's now the older-12 block, not a fiscal year) — quote what it becomes.
e) Report whether 12 + 12 + panels fits one page (legacy fits it; our earlier 24-in-one-grid also fit, so 12+12 should too).

Show the unified diff. Apply nothing until I confirm.

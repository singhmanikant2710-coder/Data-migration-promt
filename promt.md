Modify ONLY this file:
frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

Make the CRM Findings table header match the Transactions table header style 
(dark navy background with white uppercase text). Two changes:

1. Change the <thead> className FROM:
     bg-sky-200 text-slate-800
   TO:
     sticky top-0 z-20 bg-[#1F3864] text-white

2. In the local Th component in THIS file, change its className FROM:
     "box-border px-3 h-10 text-left font-semibold align-middle whitespace-nowrap "
   TO:
     "box-border px-3 py-2.5 text-left text-xs font-semibold uppercase tracking-wide align-middle whitespace-nowrap border-b border-[#1F3864] "

Keep everything else (the column names, the per-cell className overrides like 
text-center) exactly as-is.

Modify ONLY CrmFindingsAndRatingsSection.tsx. This is a styling-only change.

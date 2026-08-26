Apply the diff exactly as shown. Only edit frontend/src/blackbook/pdf/BlackBookPdf.tsx. Make exactly these two changes, in-place (replace, do not duplicate lines):

1) Change:
   const monthlyChunks = seriesYearOnly.length > 0 ? [seriesYearOnly] : [];
   to:
   const monthlyChunks = (Array.isArray(rolling24) && rolling24.length > 0)
     ? [[...rolling24].sort((a, b) => normMonthKeyInt((a as any)?.monthKey) - normMonthKeyInt((b as any)?.monthKey))]
     : [];

2) Change the FY-1 history section condition:
   {historyYearOnly.length > 0 && (
   to:
   {false && historyYearOnly.length > 0 && (
   (Replace in place — only ONE opening line for this block after the change, no duplicate.)

Nothing else changes. Apply now, then confirm both changes are in place and the FY-1 block has exactly one opening condition (no leftover duplicate).

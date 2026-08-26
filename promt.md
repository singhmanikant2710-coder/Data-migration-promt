Apply the diff exactly as shown in the preview. Only edit frontend/src/blackbook/pdf/BlackBookPdf.tsx. Make only these changes, nothing else:

1) Change:
   const monthlyChunks = seriesYearOnly.length > 0 ? [seriesYearOnly.slice(-monthlyRowsOnFirstPage)] : [];
   to:
   const monthlyChunks = seriesYearOnly.length > 0 ? [seriesYearOnly] : [];

2) Change the first history cap:
   historyYearOnly.slice(0, 6)  ->  historyYearOnly.slice(0, 12)

3) Change the second history cap:
   historyYear2Only.slice(0, 6)  ->  historyYear2Only.slice(0, 12)

Do not touch anything else — no other lines, no constants, no styles, no disabled blocks. Apply now.

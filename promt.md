Apply changes to frontend/src/blackbook/pdf/BlackBookPdf.tsx. Make ONLY these targeted replacements. Do NOT touch any closing </View> tags or braces — the JSX structure must stay exactly as it is; only change the specific strings below.

CHANGE 1 — the monthlyChunks definition. Find:
  const monthlyChunks = (Array.isArray(rolling24) && rolling24.length > 0)
    ? [[...rolling24].sort((a, b) => normMonthKeyInt((a as any)?.monthKey) - normMonthKeyInt((b as any)?.monthKey))]
    : [];
Replace with:
  const r24Sorted = (Array.isArray(rolling24) && rolling24.length > 0)
    ? [...rolling24].sort((a, b) => normMonthKeyInt((a as any)?.monthKey) - normMonthKeyInt((b as any)?.monthKey))
    : [];
  const r24Top = r24Sorted.slice(-12);
  const r24Bottom = r24Sorted.slice(0, Math.max(0, r24Sorted.length - 12));
  const monthlyChunks = r24Top.length > 0 ? [r24Top] : [];

Do NOT change the monthlyRowsOnFirstPage line or the r24Chunks line — leave them exactly as they are.

CHANGE 2 — in the bottom history block, make exactly THREE single-token edits, nothing else:
  a) Replace the condition text:  {false && historyYearOnly.length > 0 && (   ->   {r24Bottom.length > 0 && (
  b) Replace the heading text:  Historical Summary - {prevYear}   ->   Historical Summary
  c) Replace the mapped array:  historyYearOnly.slice(0, 12)   ->   r24Bottom

ABSOLUTELY DO NOT add, remove, or rearrange any </View> or ) or } characters in the closing part of that block. The closing tags must remain identical in count and order to the original. If you find yourself changing closing tags, STOP — that means you're editing the wrong thing.

After applying, show me ONLY the bottom history block from its opening { to its closing )} exactly as it now exists in the file, so I can verify the JSX bracket balance is correct. Do not run build.

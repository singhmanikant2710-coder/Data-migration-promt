Single-file edit: frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx

GOAL (Geoff comment #4): Remove the entire "Selection Summary" section from 
the report. Geoff confirmed the Current Filter Payload at the end of the 
report is sufficient, so this section is redundant.

The section is rendered on the main page via:
    {/* Selection Summary */}
    <FiltersEcho f={filters || {}} />

REMOVE this usage (both the comment line and the <FiltersEcho ... /> render) 
so the Selection Summary block no longer appears in the output.

CONSTRAINTS:
- Remove ONLY the <FiltersEcho> render (and its adjacent comment) from the 
  page layout. 
- You may leave the FiltersEcho function definition in the file unused, OR 
  remove it as well if it is not referenced anywhere else — but do NOT 
  remove it if it is used elsewhere. Check for other usages first; if the 
  only usage is the one being removed, removing the definition is fine.
- Do NOT touch any other section (header, charts, Current Filter Payload at 
  the end, detail pages).
- Do NOT remove or alter the Current Filter Payload section that appears at 
  the end of the report — Geoff wants to keep that.
- Only edit this one file. Show the changed lines.

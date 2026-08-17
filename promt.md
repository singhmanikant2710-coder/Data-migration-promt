Single-file edit: frontend/src/components/pdf/CrmSummaryTablePDF.tsx

Fix: The header banner still shows "CRM Summary Table" because the default title fallback is the old name. Geoff wants the header to read "CRM Findings Summary Table". Update the fallback string in the title computation.

BEFORE:
const title = props?.meta?.title || "CRM Summary Table";

AFTER:
const title = props?.meta?.title || "CRM Findings Summary Table";

CONSTRAINTS:
- ONLY change this one fallback string.
- Do NOT change how title is passed to CrmSummaryTablePage or anything else.
- Do NOT change the banner JSX line (its fallback can stay as-is).
- Do NOT touch any other file.
- Only edit this one file. Show the diff.

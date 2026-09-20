Small fix — the "CRM Summary for Management" report's header title text doesn't match the report name Geoff expects. He confirmed the title should read exactly "CRM Summary for Management" (it currently likely shows "Review Summary for Management", inherited from the prototype filename).

FILE: frontend/src/components/pdf/ReviewSummaryForManagementPDF.tsx

Find REPORT_TITLE (or wherever the header title text is set) and change it to exactly "CRM Summary for Management". Confirm whether this also affects the footer text ("<title> • Page X of Y") — it should, since the footer mirrors the same title constant, and Geoff would expect consistency between header and footer. Also confirm the download filename — should it also become "CRM Summary for Management.pdf" instead of "Review Summary for Management.pdf"? (Recommend yes, for consistency with the dropdown label and the other new reports' naming convention.)

Do NOT rename the file itself (ReviewSummaryForManagementPDF.tsx) or change any routing/component export names — only the displayed title text, footer text, and download filename string.

Show diff. Rebuild. Do NOT commit.

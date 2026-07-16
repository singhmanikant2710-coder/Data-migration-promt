
Read-only. No edits. No plan. Just report with file paths + code. Do NOT modify or revert anyone's existing work — this is inspection only.

UAT #65: tables and images inserted in the Customer Background comments field must render in BOTH the CAS Linesheet AND the PDF Memo reports. Currently they don't (tables flatten to one line, images dropped).

Report:
1) Confirm the component/file that generates the CAS Linesheet PDF (already known: ReviewPDF.tsx via @react-pdf/renderer). Show where it renders the comments/narrative fields.
2) Find the component/file that generates the PDF Memo (Initial Memo / Final Memo buttons on the Review Form). Is it the same @react-pdf pipeline or a different one? Show the file path and how it renders narrative/comments fields.
3) In BOTH generators, show every place stripHtml() (or equivalent tag-stripping) is applied to a comments/narrative field. List each field name.
4) Confirm @react-pdf/renderer is used for both, and that neither accepts raw HTML (so tables/images must be rebuilt as View/Text/Image components).
5) List exactly what must change, and in how many files, to render tables (row/column spacing, borders optional) and images from the comments HTML in both reports.

Use LIVE DB, ignore columns.csv. Output findings only. Change nothing. Do not alter existing logic authored by anyone.

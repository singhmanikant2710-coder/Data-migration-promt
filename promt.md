Bug 207 — CAS Linesheet PDF: when user selects "No Monitoring Covenants" / "No Performance Covenants", those rows do NOT appear in the PDF covenant tables (tables render empty). Geoff wants the "no covenant" rows to be shown. READ-ONLY, no edits. One pass, answer, STOP.

The CAS Linesheet PDF has two covenant tables: "Reporting / Monitoring Covenants" and "Financial Performance Covenants". When the review has no actual covenants (user explicitly selected "No Monitoring Covenants" / "No Performance Covenants"), the table body is empty instead of showing that no-covenant selection as a row.

Investigate:
1. Find the CAS Linesheet PDF component (likely ReviewPDF.tsx) and the two covenant tables (Reporting/Monitoring and Financial Performance). How are their rows rendered? File + line.
2. Where does the covenant data come from? Is there a distinction in the data between (a) no covenants entered at all vs (b) user explicitly selected "No Monitoring Covenants"/"No Performance Covenants"? How is that "No ... Covenants" selection represented in the data (a flag, a special covenant row, a covenant name/type)?
3. Why does the table render empty in the "No ... Covenants" case? Is the row being filtered out, or does the data simply not include a row for the "No Covenants" selection? Paste the render/filter logic.
4. How is the same "No Covenants" selection shown elsewhere (e.g. in the UI Covenants section, or another report) — is there existing precedent for displaying a "No Monitoring Covenants" row that we can mirror?
5. Identify exactly what needs to change so that when "No Monitoring/Performance Covenants" is selected, the PDF shows a row indicating that (rather than an empty table). Is the fix: include the No-Covenants selection as a row in the data, OR render a placeholder row in the PDF when the covenant list is empty-but-selected?

Report file paths + line numbers + how the "No Covenants" selection is represented in the data + why it renders empty. Do NOT fix yet.

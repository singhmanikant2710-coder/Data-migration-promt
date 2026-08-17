READ-ONLY. Diagnostics only. Do NOT change anything. Do NOT edit any file.

I need to see EXACTLY how the run date is produced and formatted in the working reports, so I can replicate it in CrmSummaryTablePDF.tsx.

Files (read each ONCE, findings only, no edits):
- frontend/src/components/pdf/CrmSummaryPDF.tsx
- frontend/src/components/pdf/CrmPdGradeMigrationPDF.tsx
- frontend/src/components/pdf/pageSetup.ts   (or wherever formatDate / formatRunDate is defined)

Show me ONLY:

1. In CrmSummaryPDF.tsx: where `generatedOn` comes from — the FULL props/type definition, and how generatedOn gets its value (default? passed from parent? fallback to new Date()?). Show the exact lines including any `generatedOn = ...` default.

2. In CrmSummaryPDF.tsx: the exact header line rendering the date — {formatDate(generatedOn as any)} — and what generatedOn holds if the parent doesn't pass it.

3. In CrmPdGradeMigrationPDF.tsx: the `formatRunDate` usage, especially the no-argument call `formatRunDate()` in DetailTablePage — show the exact line.

4. The DEFINITION of BOTH helpers:
   - formatDate (from pageSetup) — full function body, what input it expects (Date? string? ISO?), what string it returns.
   - formatRunDate (wherever defined) — full function body, including what it does when called with NO argument.

5. Confirm: when called with no real date, does either h

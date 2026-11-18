UAT #242 – CRM Findings Summary Table: bold red font when a KRI tolerance is breached

REQUIREMENT (Geoff, Issue 242 tab)
1. Apply ONLY to the CRM Findings Summary Table report.
2. Unsatisfactory Transactions: conditional formatting on the COUNT (# / %) ratio for ALL CRM components, and on the EXPOSURE ($ / %) ratio for Risk Recognition ONLY.
3. Borrower Finding Totals: conditional formatting on the COUNT (# / %) ratio only, NOT on exposure.
4. Formatting = bold, red font when the ratio breaches its tolerance.
5. Tolerances come from the Selections Library (dbo.[03_LIBRARY_09_Selections]), Tab = "CAS KRI", Selection ID = 1, one Section per metric. Geoff enters them manually in QA/Test and Production:
   RR Unsat Count 5% | RR Unsat Dollar 3% | RR Findings 10%
   SM Unsat Count 5% | SM Findings 15%
   UW Unsat Count 5% | UW Findings 10%
   CS Unsat Count 5% | CS Findings 20%
   LA Unsat Count 5% | LA Findings 10%
   (RR = Risk Recognition, SM = Scorecard Management, UW = Underwriting, CS = Credit Servicing, LA = Loan Administration.)

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- CRM Findings Summary Table end to end with file:line: reports page → service → API → repository (SqlCrmSummaryTableReportRepository or equivalent) → PDF component. How each ratio (count %, exposure %) is calculated and what the denominator is.
- Which code is shared with other reports (CRM Summary, CRM Summary for Management, CRM Sample Summary, pageSetup.ts, shared table components) – those must not change.
- Selections Library: exact column names (Tab, Section, Selection ID, Selection), data type of the Selection value, how other code reads Selections (existing repository/helper to reuse), and whether "CAS KRI" rows exist in Dev today (give me a read-only SQL).
- Does this report have any other output (Excel/CSV export, on-screen preview) that shows the same table?

PHASE 2 – PLAN (smallest generic change)
- Read the tolerances once per report run from Selections (Tab "CAS KRI", Selection ID 1), mapped by Section name to component + metric (Unsat Count / Unsat Dollar / Findings). No hardcoded tolerance values.
- Parse the value safely: "5%", "5 %", "5", "0.05" etc. – report how each form is treated; unparseable or missing → no formatting for that cell (never an error, never red by default).
- Compare the same ratio that is printed (same rounding as displayed), so a cell never looks inside tolerance but shows red, or vice versa.
- Formatting only: bold + red font on the ratio cell; values, layout, widths, header/footer unchanged. Other columns and other reports unchanged.
- Zero denominators (no transactions / no borrowers): no formatting.
- Do NOT decide silently – list options with a recommendation:
  1. Breach rule: ratio > tolerance, or ratio ≥ tolerance?
  2. Should the report also show the tolerance (e.g. a small legend/footnote "Bold red = above KRI tolerance; tolerances from Selections Library") – yes/no?
  3. Should the Selection ID filter be strictly 1, or the active/latest ID if Geoff adds a new set later (e.g. for 2027)?
  4. Dev has no CAS KRI rows: provide an idempotent scripts/sql/ insert for Dev testing only (Geoff enters QA/Prod himself) – yes/no?
- No schema change.

PHASE 3 – IMPLEMENT (only after I answer the Phase 2 decisions)
- Touch only the needed code.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- ADDED (file:line), REMOVED/CHANGED (file:line).
- Before/after table for each component and metric: value below, equal to, and above its tolerance; RR exposure; findings exposure (never formatted); zero denominator; missing/unparseable tolerance.
- Read-only SQL that returns, for a chosen sample, each component's ratios and the matching tolerance, so I can compare with the PDF.
- Confirm other reports (CRM Summary, CRM Summary for Management, CRM Sample Summary, etc.) are unchanged.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.

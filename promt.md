Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #208, Geoff) — CAS Linesheet PDF labels only (the bank is
discontinuing the term "TTBA"). In the "Deal Team" section of the CAS
Linesheet PDF, change these LABELS exactly (values unchanged):
1. "TTBA Reviewed"            → "Household Exposure"
2. "TTBA Approver"            → "RCA Approver"
3. "TTBA Approval Authority"  → "RCA Approval Authority"
"Approval Reason" stays as is.

Rules:
- Label text only. No change to data, DB columns, API fields, property
  names, variables, layout, column widths or styling.
- If a longer label wraps or overlaps its value, report it — don't restyle
  without asking.

Work in 4 phases:

PHASE 1: READ-ONLY DISCOVERY (no edits)
- file:line of the 3 labels in the CAS Linesheet PDF (ReviewPDF.tsx or
  whichever component renders it).
- List EVERY other user-visible "TTBA" / "Total To Be Approved" occurrence
  in the app with file:line and where it shows (Review Form Transactions
  section labels, Initial/Final Memo PDFs, other reports, filters, email
  bodies, Excel/CSV exports, help tips, backend messages). Do NOT change
  them — I will handle Review Form in a separate item (#209).
- If a business decision is needed, STOP and ask with A/B/C options.

PHASE 2: PLAN — smallest change: the 3 label strings in the CAS Linesheet
only. If these labels come from a shared constant also used by other
screens/PDFs, do NOT change the shared constant; scope the change to the
CAS Linesheet and tell me.

PHASE 3: IMPLEMENT — exact text and casing as above.

PHASE 4: VERIFY + REPORT — build frontend (backend only if touched).

Report:
1. What you found (file:line), plus the full list of other TTBA occurrences
   NOT changed
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. PDF before vs after (Deal Team section)
5. Edge cases: values NULL/empty (label still shows, value blank or "-" as
   today), long approver names, label wrapping
6. Other reports/memos confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged)
Do not commit or push.

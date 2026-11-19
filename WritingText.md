UAT #260 – CRM Summary report: CRM FINDINGS & OBSERVATIONS table layout

REQUIREMENT (Geoff, Issue 260 tab) – CRM Summary report run from Reports Home, table "CRM FINDINGS & OBSERVATIONS":
1. Remove the COMPONENT column from the PDF (internal report; component is clear from the Finding Code). The CRM Component MUST still be included in the Excel export of this report.
2. Move CODE to the first (left) column. Keep findings sorted ascending by Component, then Finding Code.
3. Add a SEVERITY column directly right of CODE showing dbo.[02_CORE_07_Findings].Finding_level ("Finding" / "Observation"). The column must be wide enough for "Observation" without wrapping or overlapping the TYPE column.
Final column order: CODE | SEVERITY | TYPE | DESCRIPTION.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- CRM Summary report end to end with file:line: reports page → service → API → repository → PDF component; where the CRM FINDINGS & OBSERVATIONS table is built, its column widths, and current sort order.
- Is Finding_level already returned by the query? Exact values present in Dev (incl. NULL, different casing).
- Does this report have an Excel export today? What columns does it contain?
- Which other PDFs/reports share this table component or styles (memos, CRM Findings & Observations report, CRM Summary for Management, etc.) – they must not change.

PHASE 2/3 – PLAN + IMPLEMENT (smallest change)
- Change only the CRM Summary PDF table: remove COMPONENT, CODE first, SEVERITY second, widths redistributed (DESCRIPTION gets the freed space), no wrapping of "Observation".
- Sort: Component ascending, then Finding Code ascending (report if the current order already does this).
- SEVERITY shows the stored value; NULL/blank shows empty; normalise casing only for display if values are inconsistent (report what you found).
- Excel export of this report (if it exists): keep CRM Component and add Severity; if there is no Excel export, report it and do not build one here (#256 covers exports).
- No change to other reports, shared defaults or pageSetup.ts.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- ADDED (file:line), REMOVED/CHANGED (file:line), before/after column layout.
- Edge cases: long codes, NULL Finding_level, many findings across pages (header repeats), empty table.
- Confirm other reports unchanged.
- npx tsc --noEmit, npm run build, dotnet build if backend touched.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.

Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (UAT #226, Geoff — moderate priority, needed fast): Samples page →
Select Sample grid → Create Sample (new row) and Edit row.
1. "Type" dropdown is a hard-coded list today. Source it from
   03_LIBRARY_09_Selections WHERE Tab = 'Samples' AND Section = 'Type',
   sorted by Selection_id ascending. Library options today: Continuous,
   Examination, CCL, Co-Source, Other.
2. Column header "Target (BU)" → "Target" (grid header + any label/
   placeholder on this screen only).
3. "Target" dropdown options sorted by Selection_id ascending (today they
   are alphabetical). Keep the same source; if Target is not from the
   Selections table, tell me where it comes from.
4. Add a "+ Add Target" option at the bottom of the Target dropdown. Picking
   it lets the user type a custom target for THIS sample only (one-time
   use): the text is saved on the sample row only and must NOT be added to
   the Selections library or appear in the dropdown for other samples.

Dev data (I ran these):
Selections WHERE Tab='Samples': <paste>
Existing Sample_type values: <paste>
Existing Sample_target values: <paste>

Rules / must not break:
- Existing samples whose Type/Target value is NOT in the library list must
  still display and stay unchanged when edited/saved (show the current
  value as a selectable option in edit mode).
- Find EVERY consumer of Sample type/target values (sample name
  generation e.g. "364 - 10/1/2026 - Continuous - Enterprise", Review Info
  "Sample Type", Load Samples, Review Queue/Progress filters, reports
  filters/PDFs, Excel exports, any code comparing type === '...'). If the
  library values differ in spelling/wording from what the code expects
  today (e.g. "Continuous" vs "Continuous Review"), STOP and ask me with
  A/B/C options before editing.
- Custom target: trim, required non-empty, max length = DB column length
  (validate both frontend and backend), case-insensitive match to an
  existing option → use that option instead of creating a custom one.
- No schema change. If the library is missing rows in Dev/QA/Prod, give an
  idempotent script under scripts/sql/ for the DBA (don't run it).

Work in 4 phases:
PHASE 1 (read-only): file:line for the Samples grid Type/Target controls
(create + edit), the API/service/repository/SQL that saves and lists
samples, the Selections lookup used elsewhere (reuse it), and all
consumers listed above. STOP if a decision is needed.
PHASE 2: smallest change; reuse the existing Selections API/hook used by
other screens; no duplicated lookup logic.
PHASE 3: implement.
PHASE 4: build backend + frontend; verify with real functions/data. Do not
leave harness/test files or temp DBs behind.

Report:
1. What you found (file:line) + all consumers of type/target
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen before vs after (create, edit, grid header, dropdown order,
   + Add Target flow)
5. Edge cases: library empty/unreachable; existing sample with legacy
   type/target; custom target blank/too long/duplicate of existing option/
   special chars; editing a sample that has a custom target; cancel while
   typing custom target; sample name generation with custom target
6. Other screens/reports confirmed unaffected
7. Build results
8. git status (test-data/ and artifacts flagged; no harness files left)
Do not commit or push.

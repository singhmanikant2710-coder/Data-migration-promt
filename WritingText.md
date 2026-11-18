1: A – bold red only when ratio > tolerance (equal is not a breach).
2: B – add one small footnote under the summary table: "Bold red = above KRI tolerance (CAS KRI, Selections Library)." Same font family, smaller size, no change to the table layout or other reports.
3: A – strictly Selection ID = 1.
4: A – create scripts/sql/insert-cas-kri-selections.sql (idempotent) plus a rollback script, same pattern as the existing insert-*-selection.sql scripts. Put a header comment: "DEV ONLY – Geoff enters CAS KRI tolerances manually in QA/Test and Production. Do not run in QA/Prod."
Proceed with Phase 3 and 4.

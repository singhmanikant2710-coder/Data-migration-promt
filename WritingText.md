UAT #233 – Covenants and Policy Exceptions: prevent save data loss (same safeguard as the Collateral/PSOR/SSOR rating fix, commit 15f7638).

PHASE 1 – Read-only discovery (file:line for every claim, no edits):
1. For the Covenants tab and the Policy Exceptions tab, list everything that is saved: scalar fields (CovenantsInfo, PolicyExceptionsInfo incl. Policy_variance_mitigation) AND row tables (covenant rows, policy exception rows).
2. Trace the full save path for each: frontend payload builder -> ReviewService -> SqlReviewRepository SQL.
3. Scalar fields: confirm whether EVERY column already uses the CASE WHEN @hasX = 1 THEN @p ELSE [Col] END guard from 15f7638. List any column that is still overwritten unconditionally.
4. Row tables: how are rows saved (delete-all + re-insert, merge, per-row update)? What happens when the payload has the row list undefined/missing vs an explicit empty array [] vs null? Can saving ANOTHER tab/section (e.g. RRJ, Collateral) wipe or change covenant/PE rows or fields?
5. If everything is already safe, say so with evidence and STOP – change nothing.

PHASE 2 – Plan the smallest generic fix:
- Absent/undefined field or row list = leave stored data unchanged. Explicit [] = user removed all rows, delete is correct. Same rule as 15f7638.
- No schema change. Do not touch other sections.

PHASE 3 – Implement only what Phase 2 needs. Keep the existing save transaction.

PHASE 4 – Verify and report:
- Root cause (file:line).
- ADDED (file:line) and REMOVED/CHANGED (file:line).
- On-screen before/after.
- NULL/edge cases: field undefined, null, empty string; rows undefined, [], null; saving another tab while Covenants/PE have data.
- Other sections/screens unaffected.
- Backend build + frontend build/tsc/lint results, git status.
Do not commit or push.

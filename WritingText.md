UAT #254 – Policy Exceptions: comments edited after adding a second row are lost on Save

CONTEXT
- Policy exception notes/mitigation/varianceMitigation are ONE review-level value (Reviews.Policy_exception_information, Policy_exception_mitigation, Policy_variance_mitigation), not per row (see UAT #235 work).
- Files involved in #233/#235: frontend/src/app/review/[ecif]/review-info/page.tsx (save payload builder), PolicyExceptionsSection.tsx, hooks/usePolicyExceptions.ts, frontend/src/services/api/reviews.ts, backend SqlReviewRepository.cs (PE upsert) and ReviewQueue.cs.

REPRO (Dev, unlocked review, 0 PE rows, empty comments)
1. Add row E01, type comments "AAA". Do not save.
2. Add row E02. Comments still show "AAA" (OK).
3. Change comments to "BBB", click Save, reload.
EXPECTED: "BBB". ACTUAL: "AAA".
Geoff also reports the same with previously saved comments after adding more rows later.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Trace with file:line how the comments/mitigation/varianceMitigation value is stored in frontend state: one shared value, or a copy per row?
- When the user edits comments, which state is updated (which row, the selected row, all rows, a review-level value)?
- When a row is added, how does it get its notes (#235 fix)?
- Trace the save payload: which row's / which value's notes are sent, and how many times.
- Trace the backend PE upsert: are notes written per row in a loop (last write wins)? Which value ends up in Reviews?
- State the exact root cause with file:line before changing anything.

PHASE 2 – PLAN
- Smallest generic fix: keep ONE source of truth for the review-level notes/mitigation/varianceMitigation in the frontend; every edit updates it; the save sends that single current value once (or all rows carry the same current value). No stale per-row copies can overwrite it.
- Must keep all existing behaviour:
  - #233: send only fields edited this session; an unchanged value must not overwrite stored data.
  - #235: no auto-E00; notes/mitigation locked with 0 rows + hint; unsaved-row delete stays client-side; saved row edited then deleted not re-inserted; new row shows current notes; notes kept on delete; duplicate types blocked; review-level Info object loads stored values when 0 rows.
  - Covenants tab unchanged.
- No schema change. Backend change only if the root cause is there; SQL only as idempotent scripts/sql/ (none expected).

PHASE 3 – IMPLEMENT
- Touch only the code needed.

PHASE 4 – VERIFY + REPORT
- Root cause (file:line).
- ADDED (file:line), REMOVED/CHANGED (file:line).
- On-screen before/after for: the repro above; saved comments + add row later + edit + save; edit mitigation and variance mitigation the same way; delete a row after editing comments.
- NULL/edge cases: stored notes NULL vs empty; 0 rows; user edits comments then deletes all unsaved rows; saved row deleted after comment edit; save without touching comments (stored value must not change).
- Confirm other screens unaffected (Covenants, other Review Form tabs).
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if the build rewrites them; leave the untracked scripts/sql/find-reviews-with-blanked-ratings.sql alone).
- Do not commit or push.

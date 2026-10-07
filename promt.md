Context: CASRR (.NET 8 Clean Architecture backend + Next.js/React/TypeScript
frontend + SQL Server). Production banking app: existing behaviour must not
break.

TASK (follow-up to the rating fix): Apply the same partial-update safeguard
to the two remaining single-object writers on dbo.[02_CORE_02_Reviews] that
you flagged:
- SqlReviewRepository.SaveCovenantsInfoAsync (8 columns)
- SqlReviewRepository.SavePolicyExceptionsInfoAsync (3 columns)
Today they write every column unconditionally, so a payload that omits a
field would blank it. Use exactly the pattern you used for RRJ / KeyRisks /
Scorecard / Collateral / Repayment:
  SET [Col] = CASE WHEN @hasX = 1 THEN @p ELSE [Col] END
with presence = "key present in the section payload and not JSON null"
(an explicit "" still writes ""). Backend only.

Rules:
- Do not change the frontend (including baseline merges) or the API
  contract / controller signatures.
- Do not touch the row-level collections (covenant rows, policy exception
  rows) – only the single-object section fields.
- No schema change, no new abstraction; follow the existing code style.
- Normalisers must keep absent as null (same as the NormalizeRating fix);
  non-null inputs behave exactly as before.

PHASE 1 (read-only): file:line of both repository methods, their
ReviewService callers/parsers, the exact columns, and what the frontend
currently sends for each section (full object or partial).
PHASE 2: plan the smallest change.
PHASE 3: implement.
PHASE 4: build backend + frontend (frontend only as regression check);
verify with the real ReviewService.SaveAsync (mocked repository is fine,
delete any harness afterwards): partial payload keeps other columns;
full payload saves all; explicit "" clears; JSON null treated as absent.

Report:
1. What you found (file:line) + current payload shape per section
2. ADDED lines (file:line)
3. REMOVED / CHANGED lines (file:line)
4. On-screen behaviour before vs after (Covenants and Policy Exceptions
   saves – should be identical for today's full payloads)
5. NULL / edge cases: field absent, "", JSON null, already-NULL column,
   two sections saved together, locked/approved review path
6. Other sections confirmed unaffected
7. Build/test results
8. git status (no harness/temp files)
Do not commit or push.

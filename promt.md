Task: APPLY Hunk A only (approved). Hunk B = diff only, DO NOT apply.
Do NOT commit, stage, push, or create branches. Check-in will be manual.

STRICT RULES (do not break anything):
- Edit ONLY backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs,
  only inside TryMergeTtmIntoSeries, lines ~2130-2135.
- Change exactly this, nothing else:
  1. Replace the comment on line 2130 with the corrected comment
     (missing-only fallback; a zero YTD is a real value).
  2. Delete the line: var isZero = (hasYtd.HasValue && Math.Abs(hasYtd.Value) <= 0);
  3. Change: if (!hasYtd.HasValue || isZero)  ->  if (!hasYtd.HasValue)
- Do NOT touch: the hasYtd line itself, the TTM lookup (2137), the write
  at 2139, the four alias mirrors (2141-2144), the try/catch, or any
  other method, file, using-directive, or formatting.
- Preserve the file's existing line endings (CRLF/LF), encoding, and
  indentation exactly. No auto-format, no whitespace cleanup, no
  reordering of usings.

AFTER APPLYING, report:
1. Raw output of: git diff -- backend/src/Bcat.Infrastructure/SqlServer/SqlMainRepository.cs
   Paste it exactly as the terminal printed it. Do not retype or summarize.
   The diff must show ONLY: the comment change, the removed isZero line,
   and the changed if-condition. If any other line appears (including
   the hasYtd line due to whitespace), revert that line and re-run git diff.
2. Raw output of: git status   (confirm only this one file is modified)
3. Build result of the backend solution and of Bcat.Infrastructure.Tests
   (pass/fail + error count). Bcat.Api.Tests has 56 pre-existing errors;
   confirm the count is still 56, not higher.
4. A plain-English summary (3-4 lines): what changed, and what behaves
   differently after the fix:
   - stored YTD = 0          -> now shown as 0 (before: replaced by TTM)
   - YTD missing, TTM exists -> still TTM (unchanged)
   - YTD non-zero            -> unchanged

HUNK B - DIFF ONLY, DO NOT APPLY:
File: frontend/src/blackbook/mappings/manufacturing.ts, lines ~207-212.
Minimal change only:
  - Update comment on line 207 to: stored YTD (a zero is a real value)
    -> if MISSING: TTM PBT -> computed YTD sum
  - Remove: const hasNonZeroYtd = yNum !== null && Math.abs(yNum) > 0;
  - Change: if (!hasNonZeroYtd) {  ->  if (yNum === null) {
  - Update comment on line 212 to say only a MISSING YTD falls back.
Keep alias lists, TTM-then-sum ordering, braces, and everything else
byte-for-byte unchanged. Show the proposed change as a unified diff,
and also quote lines 207-223 verbatim from the CURRENT file (copy, do
not retype) so the alias lists can be checked.

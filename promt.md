Apply the 4-line fix. Rebuild, confirm typecheck/build pass. Show diff. Do NOT commit.

FILE 1: frontend/src/app/api/monthly-upload/parse/route.ts
- Line 105: 30-59 → 31-59 in DELINQUENT_ALLOWED regex
- Line ~133: 30-59 → 31-59 in error message string

FILE 2: frontend/src/app/api/monthly-upload/save/route.ts
- Line 152: 30-59 → 31-59 in DELINQUENT_ALLOWED regex
- Line ~180: 30-59 → 31-59 in error message string

Leave the \s* (permissive whitespace matching for "NonAccr") as-is — not changing to \s+.
Do NOT touch the ExportsController.cs comments (cosmetic only, zero behavior impact) or SqlSampleLoadRepository.cs (unrelated, doesn't reference 31-59).

Show diff. Rebuild. Do NOT commit.

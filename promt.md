Q1: A — Block with 400 error, mirroring the existing "Header mismatch" pattern exactly. Simple, safe, reuses the existing error channel, zero new UI needed.

Q2: A — parse.ts + save.ts only. Do not touch page.tsx. Since Save depends on a successful parse, gating there covers both CSV and xlsx uploads with the smallest possible change.

Note: Geoff confirmed this data mart refresh is a monthly external process (owned by Jothi/Jessica), so they will also manually verify source-file cleanliness going forward. This code change is a safety net for future uploads through OUR pipeline, not a fix for the already-corrupted 1,248 rows (that's a separate one-time data correction, not in scope here).

Proceed with the validation as scoped: reject any DelinquentID value that doesn't match the known domain (curr, 1-30, 30-59, 60-89, 90+, and NonAccr-prefixed variants) or looks date-shaped, with a 400 error listing the bad row(s) — same UX as the existing header-mismatch block.

Your analysis is likely correct — the slowness is probably the ORDER BY [Review_id] I added, not StripHtml. Investigate and fix the REAL cause. READ-ONLY first, then fix.

READ-ONLY:
1. Is Review_id the clustered index / primary key of 02_CORE_02_Reviews? Check. If it IS the clustered key, ORDER BY Review_id is nearly free (data already in that order) — then ORDER BY isn't the cause. If it's NOT clustered, sorting 150k rows that include nvarchar(max) rich-text columns (base64 images) forces a massive sort spilling to tempdb — that's the 283s.
2. Confirm with the actual query cost: was the export fast on local BEFORE the ORDER BY was added? The original was SELECT * WITH (NOLOCK) with NO order by — that's what deployed still runs (fast). We added ORDER BY.

FIX:
- The original had WITH (NOLOCK) and no ORDER BY. Geoff's bug #222 was about (a) HTML jargon and (b) encoding — NOT about row order. Row order was my over-engineering.
- Option 1 (simplest, safest): REMOVE the ORDER BY entirely. Keep the original ordering behavior (no explicit order). This eliminates the sort cost. Also decide on NOLOCK: the original had it; removing NOLOCK was also my addition. If NOLOCK removal isn't required for the bug, restore the original (SELECT * FROM ... WITH (NOLOCK)) with NO order by — matching the fast deployed version — and keep ONLY the StripHtml + BOM changes which are the actual bug fix.
- So the SQL should go back to the original fast form; only StripHtml (HTML strip) + BOM (encoding) are the real #222 fixes.

Confirm: is Review_id clustered? And show the SQL reverted to no-ORDER-BY (original fast form) while keeping StripHtml + BOM. Do NOT commit.

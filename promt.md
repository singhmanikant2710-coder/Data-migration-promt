Bug 222 — the CORE Reviews export now works and produces clean output, but it's VERY slow (StripHtml regex over ~150k rows × 11 rich-text columns, with large base64 <img> data URIs). Optimize StripHtml for linear performance WITHOUT changing its output. Show diff, do NOT commit.

FILE: ExportsController.cs — StripHtml only.

1. Paste the current StripHtml. Identify the slow part — likely a greedy/backtracking regex (e.g. <img.*> or repeated Regex.Replace passes) that goes pathological on long base64 strings.
2. Rewrite for linear time:
   - Use static compiled regexes (Regex with RegexOptions.Compiled, declared once as static readonly fields — NOT recompiled per cell/row).
   - img removal: use <img\b[^>]*> (linear — base64 data URIs contain no '>' so [^>]* won't backtrack). NOT <img.*>.
   - tag removal: single pass <[^>]+> (linear).
   - Add a fast pre-check: if a cell has no '<' and no '&', return it unchanged immediately (skip all regex) — most cells aren't HTML, so this alone cuts most of the cost.
   - Do entity decode (WebUtility.HtmlDecode) and whitespace-collapse only when needed.
   - Ensure a bounded number of passes (no nested/loop replaces).
3. Keep the exact same cleaned output (img removed, tags removed, entities decoded, whitespace collapsed, trimmed).

Show current vs optimized StripHtml. I'll rebuild and re-test timing + output.

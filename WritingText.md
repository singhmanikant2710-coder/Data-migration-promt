Follow-up (same task, do not commit): Yes – apply the same protection to the three under-protected sinks you reported:
- CrmFindingsAndRatingsSection.tsx ~:678 and ~:782
- TransactionsSection.tsx ~:1330

Rules:
- Keep the existing sanitizeRichHtml link behaviour exactly as today. Wrap it directly in the sink as:
  DOMPurify.sanitize(sanitizeRichHtml(value ?? ""), RICH_HTML_SANITIZE_CONFIG)
  Do not remove, rename or refactor the existing helper.
- Reuse the same import lines and RICH_HTML_SANITIZE_CONFIG from @/lib/htmlSanitize. No new config, no new dependency.
- Confirm these sinks are client-only (or render after mount) so sanitize never runs on the server; if any of them can render during SSR, STOP and tell me before changing it.
- Touch only these 3 lines (+ imports if missing). No other change.

Verify:
- Real saved CRM finding comments and transaction text (incl. tables, images, links) – zero visual change (compare tags/attributes before vs after like you did for rrj-20120).
- The same malicious samples (<img onerror>, <script>, javascript: link, <svg onload>, <iframe>, style url(javascript:)) are removed.
- NULL / empty → blank, no crash.

Report: CHANGED lines (file:line), before/after, SSR check, tsc + npm run build results, git status (no harness/temp files). Do not commit or push.

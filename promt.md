Q1: A — Restore then auto-save. It reuses the existing handleSave + refetch path exactly, no section-hook rewiring (Option C is too risky against the no-break requirement).
Q2: A — localStorage, 24h TTL, keyed by ecif|reviewId|sampleId, deleted immediately on successful Save/Cancel/Discard. (sessionStorage fails browser-close + session-expiry criteria.)
Q3: B — Pure-logic tests using built-in node --test, no new dependencies. Keep the draft store as a dependency-free module. Do NOT add Vitest/RTL or any devDependency (violates AGENTS.md / no-break rule).

Additional constraints (critical):
- Do NOT modify any existing section hook, the existing handleSave, or any current Review Form behavior when there are no unsaved changes.
- The draft/autosave layer must be ADDITIVE — a separate module that wraps around the existing save flow, not a replacement.
- Debounce autosave (e.g. 2-3s idle) so it doesn't spam the API. Prefer local-draft-only autosave (localStorage), NOT server autosave, unless the repo already has a draft API — check first; if a server draft API exists, reuse it, otherwise local draft only.
- No DB/schema changes.
- Show me the full plan + all diffs BEFORE applying. Do NOT auto-apply. This touches the most critical screen (Review Form), so I need to review every change first.

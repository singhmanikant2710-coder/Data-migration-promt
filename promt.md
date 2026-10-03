Answers:
1. A — the "Review Queue" button (TopChromeBar.tsx:187-203) is the one. Leave "Home" (/) untouched.
2. B — keep same element/position/styling; label, aria-label and title reflect the real destination ("Review Progress" / "Review History"). With no/invalid returnTo, everything stays byte-identical to today ("Review Queue").
3. A — seed selectedSample from sessionStorage before the first fetch and send it in that first request; keep the server-echo logic untouched. If the server echoes a different sample than the one requested, do not add pinning logic — report it to me with file:line.
Continue with PHASE 2 onward.

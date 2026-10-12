Yes – do the safe part of item 1 now (separate small change, do not commit):
- In monthly-upload/save/route.ts final catch (~:644–647): stop returning `stack` to the browser; log err.message + stack server-side only (console.error with a correlation id), and include that correlation id in the JSON response.
- Keep `error: err.message` exactly as today for now, because the UI relies on it for the header / DelinquentID / code-validation messages (:569, :584, :594). Do NOT change those messages.
- No other change; do not touch item 2 or any .NET code.
Verify with the real route: validation errors still show the same message in the UI; a simulated backend 500 no longer returns a stack; tsc + npm run build pass. Report CHANGED lines (file:line), before/after JSON, git status. Do not commit or push.

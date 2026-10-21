UAT #253 – Email modal: allow multiple attachments (currently only one)

ISSUE (from UAT)
The email modal opened by the Email button allows only one attachment. Users sometimes need to attach 2 or more files.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Find every Email button and the email modal component(s) with file:line. Is it one shared modal or several?
- How is the attachment picked today (input type=file, single?), stored in state, validated (type, size), and sent (FormData / base64 / JSON)? file:line for each.
- Does the modal also auto-attach a generated file (e.g. memo/Linesheet PDF)? How does that interact with the user's attachment?
- Trace the API route (Next.js route and/or .NET controller/service) and the actual email sender (SMTP, Graph, other) with file:line. Does it accept a single attachment or a list?
- Existing limits: per-file size, total size, allowed file types, request body size (Next.js route config, Kestrel/IIS limits, mail server limit). Report the values and where they are set.
- Any email audit/log record that stores the attachment name — single field or list?
- Security: is the file type/size validated on the SERVER, not just the browser? Filename sanitisation? Any Veracode-relevant pattern (path traversal, content-type trust, no size limit)?
- State exactly what blocks multiple attachments today (frontend only, or backend too).

PHASE 2 – PLAN
- Smallest generic change: allow selecting/adding multiple files, show a list with file name + size and a remove (x) per file, send all files, backend attaches all of them.
- Keep the current single-attachment flow and any auto-attached PDF working exactly as today.
- Keep or add SERVER-side validation for every file: allowed types, per-file size, total size, safe file names. Do not trust browser checks alone.
- Do NOT pick limits silently. Propose a max file count and a max total size based on the existing limits/mail server limit, and list them for me as a decision before implementing.
- Duplicate file names: propose behaviour (allow / rename / block) for me to decide.
- No schema change unless the audit/log stores attachments and must store several; if so, explain and ask first. SQL only as idempotent scripts/sql/.

PHASE 3 – IMPLEMENT (after my answers on limits/duplicates)
- Touch only the code needed.

PHASE 4 – VERIFY + REPORT
- Root cause / what blocked multiple attachments (file:line).
- ADDED (file:line), REMOVED/CHANGED (file:line).
- On-screen before/after: picking 1 file, 3 files at once, adding files in two separate picks, removing one, sending with no user file (auto-attached PDF only, if any).
- NULL/edge cases: zero files; a file over the per-file limit; total over the limit; disallowed type; 0-byte file; same file picked twice; very long or non-ASCII file names; network error mid-send (no partial email).
- Error messages shown to the user must be clear and must not leak stack traces or server paths.
- Confirm all other Email buttons/screens still work.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave untracked scripts/sql/find-reviews-with-blanked-ratings.sql alone).
- Do not commit or push.

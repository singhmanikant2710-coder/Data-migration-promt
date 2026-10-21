Q1: A – max 5 files, 10 MB total (per-file 5 MB unchanged). Enforce on both browser and server.
Q2: A – block duplicate file names (case-insensitive), toast "X is already attached"; server rejects too.
Q3: A – reject 0-byte files with "X is empty" in browser and server.
Q4: 1, 2 and 3 – sanitise file names, PDF content check (.pdf + %PDF- header, send as application/pdf), and remove RawError/stack traces from ProblemDetails (keep server-side logging).
Proceed with Phase 3 and 4.

Accepted, two small follow-ups:
1. TOP(500) cap: if more reviews match than the cap, do not truncate silently – show a clear message ("N reviews match; the report is limited to 500. Please narrow the filters.") and do not generate a partial report. Same for the Excel export.
2. Start/End dates without a Review Status must not count as a filter for the gate (they don't filter anything). Dates with a status count as before.
Report CHANGED (file:line), dotnet build, npx tsc --noEmit, npm run build, git status. Do not commit or push.

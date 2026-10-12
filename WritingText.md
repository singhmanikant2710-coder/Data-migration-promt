Go ahead with option C.
- Tighten ensureRelativePath in lib/navigation.ts: reject any backslash, tab, newline or other control character, and a leading "//" (after trimming), falling back to "/" as today. No other behaviour change.
- Use it in the three section copies (ReviewInfoSection ~:80, TransactionsSection ~:132, CrmFindingsAndRatingsSection ~:137): one line + one import each. No other edits.
- Verify: real section tabs, returnTo=queue/progress/history, Save and deletes produce byte-identical URLs before vs after (also for page.tsx #43/#44/#62 call sites); the full hostile matrix shows 0 off-site results; tsc + npm run build pass.
- Then write ready-to-paste Mitigate by Design text for #65 and #66 (TSRV format: Technique, Specifics, Remaining Risk, Verification) with the new line numbers, describing the hardened validation.
- Delete %TEMP%\cassr-cwe601 at the end. Report ADDED / CHANGED lines (file:line), results, git status. Do not commit or push.

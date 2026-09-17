Cross-check the DELINQUENT_ALLOWED validation regex/allow-list in parse/route.ts and save/route.ts against Geoff's confirmed complete domain list:

1-30, 31-59, 60-89, 90+, curr, NonAccr 1-30, NonAccr 31-59, NonAccr 60-89, NonAccr 90+, NonAccr Curr

Paste the EXACT current regex/allow-list from both files. Specifically confirm: does it say "30-59" or "31-59"? Check character-by-character — this determines whether legitimate "31-59" values will be incorrectly blocked on the next real upload.

If there's a mismatch (code has "30-59" instead of "31-59", or any other discrepancy against Geoff's list), fix it in both files. Show diff, do NOT commit. Rebuild and confirm typecheck/build pass.

If the code already exactly matches Geoff's list, just confirm that clearly — no changes needed.

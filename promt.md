Redo this round cleanly (my earlier Reject may have left partial state).
Rounds 1-3 stay as they are.

1. Remove display carry-forward for covenant VALUES at all 6 sites you
   listed, including REG ~2009 (keep it, you were right).
2. NULL covenant actual must render "—": add a covenant-scoped null
   guard at the render sites. Do NOT change formatCurrency.
3. Seeding (legacy parity): in SqlCovenantRepository.SeedFromLatestAsync,
   do NOT copy strCovenantActual into the new month — insert it as NULL.
   Keep copying name, reported, order, threshold, description, format
   from the latest month as today. Do not change the seed source.

Build, tests, do not commit. Report files + lines.

Hi John, trying to close everything out since we need to deliver this tomorrow — quick recap of where things stand + a few things I need from you to finish up.

Where we are:
- The core save-path bugs (PK violation, fiscal month/year drift, covenant write-target, elapsed-days formula) are fixed, verified, and committed.
- The fiscal-start-change cascade (Option b, full recompute, including intElapsedFiscalDays) is designed and mostly ready — held back only by the datFiscalYearStart question below, since the same calculation feeds both.
- On datFiscalYearStart: we found the column disagrees with itself across ~30 fiscal years in the current data (same fiscal year, two different start dates among its own rows) — so this isn't just a labeling-convention question anymore, there's real inconsistency in what's stored today regardless of which definition is correct.

Three things I need from you:

1. Status on the legacy fix for the 4 exception customers (Bankers Healthcare, Keystone, Nationwide, Vermeer) — has that started, and do you have a rough timeline? Want to know if we should expect it before or after we ship our side.

2. Scope on that same fix — I'd asked earlier and want to make sure it didn't get missed: does "fix the data in legacy" mean the Access database only, or will your team also correct these 4 customers' rows in the SQL Server dev/prod database we've been working in? If it's Access-only, we'll need to run our own backfill there to match once yours lands.

3. Still waiting on your read on the datFiscalYearStart question I sent — whether it should mean "first day of this row's fiscal year" (we fix + backfill) or something else legacy reporting depends on. This is the one blocking piece left on our side — once I have that, I can close out both the column fix and the cascade in the same pass.

Also — is there anything else on your end that could affect what we're shipping tomorrow? Want to make sure we're not missing a blocker before we finalize.

Thanks for the detailed analysis on all 3 open questions. Here are the decisions —
please proceed with all of this together as one piece of work:

1. RM/PM EMAIL RESOLUTION:
Go with matching the selected RM/PM name against
[03_LIBRARY_10_Distribution Parties].Recipient_name to resolve Recipient_email
(the Data Mart Trial dropdown source stays unchanged — do NOT switch RM/PM to
Distribution Parties, and do NOT touch the existing RM/PM queries).

This must NOT fail silently:
- If no matching name is found in Distribution Parties, still persist the name +
  employee ID as usual, leave the email field NULL, and log a server-side warning
  noting the unmatched name — do not throw or block the save.
- Add a code comment flagging that this is a name-based match and is fragile to
  formatting differences (e.g. "LEO MUTCHLER" vs "MUTCHLER, LEO") — accepted
  limitation for now, not a bug to swallow silently.
- After implementing, test against real data and tell me which names (if any)
  fail to match, so I can flag it to Geoff as a data-quality follow-up.

2. PORTFOLIO MANAGER LEAD EMAIL COLUMN:
Before concluding the column doesn't exist, check the C# entity model / EF
migration files for dbo.[02_CORE_02_Reviews] in this codebase first — the
existing Portfolio_mgr_lead_name field mapping should already be defined there,
and a sibling email column (if it exists) is likely mapped nearby.

If that's inconclusive, do NOT request live DB credentials or ask me to run
az login. Instead, give me a single INFORMATION_SCHEMA.COLUMNS query (same
approach we've used throughout this project) and I'll run it and paste you the
result.

If it turns out no such column exists: do not guess a name or add one yourself.
Persist Portfolio Manager Lead name only, add a // DEVELOPER: comment noting the
missing email column, and give me the exact ALTER TABLE statement I'd need to run
to add it — I'll review and run it separately, same as we did with the Selections
script.

3. DROPDOWN VALUE FORMAT:
Keep the display string as the value: RM/PM continue sending "ID - NAME" exactly
as today (backend keeps using the existing SplitNumberName logic — do not touch
it), and PML/ECO/SCO continue sending plain name. Add a hidden companion
field/value alongside each of the 5 dropdown fields specifically to carry the
resolved email through to save. This is the smallest diff and avoids touching any
already-working parsing logic.

Apply all three decisions together, then give me a summary of what changed and
flag anything (like unmatched RM/PM names, or a missing PML email column) that
needs my input before this is considered complete.

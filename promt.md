Hi Ashok,

Agreed — let's apply it along with the DB CTASK in Prod.

On "which name will be considered": with a unique index on Recipient_role,
there is never a choice to make. The table can only ever hold one row per
Employee ID, so a second row with the same ID (different name or email) is
rejected at insert/load time.

So the correct name has to be decided in the source data before the load,
not by the database or the app. Geoff has already done this: he matched the
list against the full FHN Associate list to identify the true
Name/Email/ID for each person.

If a duplicate ever slips into a future load, the index will fail that
insert. Someone then corrects the source row, rather than the app silently
picking one of the names.

Also, since '0' would repeat for every row without an ID, please either
default Recipient_role to NULL or make the unique index filtered
(WHERE Recipient_role IS NOT NULL AND Recipient_role <> '0'). Otherwise
the index will reject every row after the first one without an ID.

Thanks,
Manikant

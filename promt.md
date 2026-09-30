Hi Ashok,

1) Default and non-numeric values:
Agreed, DEFAULT ('0') is char, so that part is fine. Two remaining points:
- '0' doesn't match the 5-char convention (existing rows use '00000'), so
  we'd have two encodings for "no ID". Suggest DEFAULT NULL instead. NULL is
  also what our sample-load writes when there's no match, so it keeps the
  data consistent.
- On non-numeric values: the app itself only ever writes digits (the UI
  enforces digits-only, max 5), so it can't create a non-numeric value. The
  risk is a value entered directly in the DB. In that case:
  (a) the trigger's "<> 0" comparison forces an int conversion and would
      error on that row, and
  (b) our API reads these columns with Convert.ToInt32, which would also
      fail on a non-numeric value.
  So yes, agreed: we'll change these properties to string in the
  application so it no longer depends on the value being numeric. Please
  also change the trigger comparison to <> '0' (string).

2) Unique key:
A composite unique on (name, email, role) wouldn't catch the problem Geoff
found. The same Employee ID with two different names or emails would still
be three unique combinations, so it would be allowed. Our resolver needs
the ID itself to be unique.

Suggestion:
- Keep Recipient_email as the primary key (as today).
- Add a unique index on Recipient_role alone. If NULLs are allowed, make it
  filtered so multiple NULLs are permitted:
  CREATE UNIQUE INDEX UX_DistributionParties_RecipientRole
  ON dbo.[03_LIBRARY_10_Distribution Parties] (Recipient_role)
  WHERE Recipient_role IS NOT NULL;
- Note: if the default stays '0', every row without an ID would get '0' and
  violate this index. That's another reason to default to NULL.

Please apply this in Prod before Friday's data load, so any duplicate ID in
the 1,495 rows is caught at load time.

Thanks,
Manikant

Thanks, Geoff. We'll keep email as the primary key and have Ashok add a
unique index on Recipient_role (Employee ID) as well, so a duplicate or
errant ID gets rejected at entry. I've asked him to apply it before
Friday's migration, so any duplicate in the 1,495 records is caught at
load time.

Thanks,
Manikant


Add server-side validation in DistributionPartiesController Create and Update:
Role must be non-empty, digits only, max 5 characters. Return 400 with a
clear message otherwise. Report ADDED/REMOVED (file:line), behaviour for a
valid ID (must be unchanged), the blank/NULL Role case, and the build result.
Do not commit.

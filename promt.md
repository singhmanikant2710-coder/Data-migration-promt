Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. Update the Distribution Parties maintenance screen (Library
Maintenance section — the screen showing EMAIL (KEY) / NAME / ROLE columns with
an "Add Distribution Party" button and Edit/Delete actions per row).

IMPORTANT: Only touch this maintenance screen's Add/Edit form and its
save/list logic. Do not touch the RM/PM/PML/ECO/SCO dropdown work, the email
resolution logic, or any other screen already completed.

Background: Recipient_role in dbo.[03_LIBRARY_10_Distribution Parties] has been
repurposed (confirmed by Geoff and the DB team) to store Employee ID instead of
a role-type string. The maintenance screen's Add/Edit form currently has a Role
dropdown offering ECO / PML / RPML / SCO / CCE, which writes a role-type string
into Recipient_role — this is now wrong and will corrupt the join key for any
record added or edited going forward.

Tasks:

1. Replace the Role dropdown in the Add/Edit Distribution Party form with an
   Employee ID input field:
   - Numeric input (validate it's a valid integer — match the format used in
     Data Mart Trial's OfficerNumber / PM Number, e.g. no letters, no
     decimals).
   - Required field (same as Role currently is), since email resolution
     depends on every row having a valid ID.
   - Field label: "Employee ID" (or whatever this project's existing
     convention is for similar ID fields — check how Employee ID is labeled
     elsewhere in the app, e.g. the RM/PM dropdown's "48191 - LEO MUTCHLER"
     format, for label/style consistency).

2. Update the list table's "ROLE" column:
   - Rename the header to "EMPLOYEE ID" (or similar) and display the numeric
     value instead of a role string.
   - Keep search functionality working (the existing "Search email, name, or
     role..." box) — update it to search by employee ID instead of role
     string, or confirm it already does a generic text search that will work
     unchanged.

3. Validation on save:
   - Employee ID must be a valid integer.
   - Do not allow duplicate Employee IDs unless the existing behavior already
     permits duplicate roles — check current duplicate-handling logic and keep
     it consistent, just applied to the new field.

4. This is forward-looking only — do NOT attempt to migrate/backfill existing
   records' Recipient_role values from role-strings to employee IDs. That's
   covered separately by the DB team's full-table repopulation (clearing and
   reloading with the 930-user file). Existing rows in this screen's list may
   still show old role-string data until that repopulation lands — that's
   expected and fine.

Acceptance criteria:
- Add Distribution Party form now captures Employee ID (numeric, required)
  instead of Role.
- Edit form for existing records shows the new Employee ID field (existing
  role-string values, if edited, get overwritten with a real employee ID going
  forward — don't try to auto-convert them).
- List table displays/searches by Employee ID.
- No other screen or the RM/PM/PML/ECO/SCO dropdown feature is affected.

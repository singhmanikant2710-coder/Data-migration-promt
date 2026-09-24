QUESTIONS FOR GEOFF:

1. RM/PM dropdown data source — final confirmation
Your original spec says RM/PM dropdowns should query Distribution Parties
directly. We implemented Data Mart Trial + Employee ID join instead
(matching on Recipient_role). Is this approach acceptable, or do you want it
changed to query Distribution Parties directly?

2. Leading zeros — do you actually need them?
Recipient_role, Relationship_mgr_number, and Portfolio_mgr_number are all
declared as int today, so leading zeros (e.g. 00030) are lost. Is preserving
them actually needed for any business or reporting reason? (If yes, we'll
ask the DBA team what it takes to change the column types.)

3. Missing PM records
You mentioned the last two PMs from the unmatched list were new and you'd
add them to Distribution Parties yourself — has that been done?

4. Gap analysis follow-up
We're running the SourceSystem-filtered (ACBS/MWS/IFL) unmatched RM/PM
report with Committed Exposure subtotals you asked for — want us to send it
as a report, or review it live now if it's ready?

---

QUESTIONS FOR JOHN / ASHOK (DBA TEAM):

1. Leading zeros — feasibility (only if Geoff confirms it's needed)
If leading zeros need to be preserved: how much effort is it to change
Recipient_role, Relationship_mgr_number, and Portfolio_mgr_number from int
to varchar/nvarchar? Can it be bundled into the reload you're doing right
now, or does it need a separate pass?

2. Unique constraint on Recipient_role
Since you're already updating Distribution Parties — can you add a unique
constraint on Recipient_role? No duplicate Employee IDs exist today, but a
DB-level constraint would prevent one from being introduced later and
silently breaking our email-resolution logic, which depends on this column
being unique.

3. Scope and timeline of the current reload
Is this update to Distribution Parties happening now for Dev/QA only, or
does it also cover next week's production migration? When do you expect
this to be complete, so we can plan our QA push accordingly?

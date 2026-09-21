Hi Geoff,

Quick update on today's progress for the RM/PM/PML/Credit Drop-Downs item, plus a
few things I need to confirm before we push to QA.

WHAT WE COMPLETED TODAY:
- Relationship Manager, Portfolio Manager, Portfolio Manager Lead, Executive
  Credit Officer, and Senior Credit Officer dropdowns on the Review Form's
  Customer Info page now point to the Distribution Parties table.
- Removed the Recipient_role filter that was previously restricting the
  PML/ECO/SCO dropdown results.
- Added search-as-you-type to all 5 dropdowns (matching the existing Reporting
  page filter search UX).
- RM/PM email is now resolved by joining on Employee ID (Recipient_role),
  since that column has been repurposed to store Employee ID per your
  confirmation. Name, Employee ID, and Email are stored on the review record
  when RM/PM are updated; Name and Email are stored for PML/ECO/SCO updates.
  A safety net logs a warning (non-blocking) if a resolved record's name
  doesn't match the selected name, so we can catch any future data anomalies.
- Updated the Distribution Parties maintenance screen: the Add/Edit form's
  Role dropdown (ECO/PML/RPML/SCO/CCE) has been replaced with an Employee ID
  field, so records added or edited going forward stay consistent with the
  new schema. This was tested against the 930-record reload and is working
  correctly.
- All of this is merged to develop (commit 799445c).

QUESTIONS FOR YOU:

1. Portfolio Manager Lead email — does dbo.[02_CORE_02_Reviews] have an email
   column for Portfolio Manager Lead? This is blocking the Email Functionality
   task (item 4) from being completed. If it doesn't exist yet, we'll need it
   added.

2. Your original spec doc (items 6-7) describes the RM/PM dropdowns querying
   Distribution Parties directly, with Employee Number and Email pulled
   straight from that table. What we've implemented instead keeps RM/PM
   sourced from the Data Mart Trial dropdown (as today) and resolves the
   email separately via an Employee ID join to Distribution Parties. Is this
   approach acceptable, or do you specifically want RM/PM to be sourced
   directly from Distribution Parties going forward? Want to align on this
   before QA so we're not doing rework later.

3. Distribution Parties maintenance screen — can you confirm the column
   order/layout is fine as-is (EMAIL / NAME / EMPLOYEE ID), or would you
   prefer Employee ID displayed before Name?

FOR THE DBA TEAM:
Can you confirm whether the Distribution Parties table repopulation (clearing
and reloading with the 930-user file, Recipient_role repurposed to Employee
ID) has been completed in the Dev and Test/QA environments? Our code changes
are environment-agnostic and will pick up the refreshed data automatically,
but we want to confirm the reload has landed before we push this to QA so
testing reflects the correct data.

Once we hear back on the above, we'll push this to QA.

Thanks,
Manikant

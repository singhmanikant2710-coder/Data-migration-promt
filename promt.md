Bug 223 — CAS Findings library edit fails with PRIMARY KEY violation. READ-ONLY, no edits. One pass, open all below, answer, STOP.

ERROR (from UAT): "Violation of PRIMARY KEY constraint '03_LIBRARY_01_CAS Findings$PrimaryKey'. Cannot insert duplicate key in object 'dbo.03_LIBRARY_01_CAS Findings'. The duplicate key value is (CRM-00)." Occurs when editing an existing CAS Finding (Component "02-Scorecard Management", Finding Code CRM-00) and clicking Save.

This is the same family as the Selections PK bug we fixed. Trace the CAS Findings maintenance edit path:

1. Frontend CAS Findings maintenance page (search app/maintenance for "findings" / "CAS Findings" / the page with Component, Finding Code, Category, Description, Guidance, Finding Group, Active columns). Find the Edit Save handler. Report: does it call an update service or a create service on save? Paste the exact call + payload keys. Does it send a query param (like the section fix), and is any required field missing on edit?

2. Backend FindingsController.cs — find the PUT/update endpoint for a CAS Finding. Report: verb, route, [FromQuery] params, the DTO bound, and which repo method it calls (Update vs Create/Add).

3. SqlFindingsRepository.cs — open the Update method AND the Create method. Report:
   - The exact PRIMARY KEY of table dbo.[03_LIBRARY_01_CAS Findings] (search for CREATE TABLE / PK / the key columns; is it Finding_Code alone, or composite like (Component, Finding_Code)?).
   - Does Update do a real UPDATE (matching on the PK), or does it delete+insert / call Create? 
   - In the WHERE/match clause of Update, which columns are used? Does the match include a field being edited?
   - Is there any INSERT that reuses a client-supplied Finding Code that could collide?

4. State the root cause: is edit firing an INSERT instead of UPDATE, or is Update matching on the wrong key so it inserts a duplicate (CRM-00)? Confirm with evidence.

Report file paths + line numbers + the exact PK definition. Do NOT propose or write a fix yet.

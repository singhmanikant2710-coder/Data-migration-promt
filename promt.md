READ-ONLY. Do NOT edit. Report only.

I need to integrate Help Tips into 5 review tabs (Key Risks, CRM Findings, CRM Ratings, 
Risk Rating Justification, Checklist). Help tips live in the maintenance help-tips 
feature backed by dbo.[03_LIBRARY_06_Help Tips].

Report ONLY:

1. Show the schema of dbo.[03_LIBRARY_06_Help Tips] — its columns (e.g. Form, Topic, 
   HelpTip). Give a read query to see sample rows in SSMS.

2. How does the existing /maintenance/help-tips screen read this data? Show the backend 
   read method and the frontend component that lists help tips. What identifies a tip — 
   is it keyed by Form + Topic?

3. Is there ANY existing help-tip display in the review form (e.g. an (i) icon that 
   already fetches a tip by Form/Topic)? Or are the (i) icons currently just static 
   tooltips with hardcoded text? Show one example from a review tab.

4. Is there a backend API endpoint to fetch a help tip by Form/Topic (for the review 
   form to call), or only the maintenance CRUD? Show the controller/route.

5. For the 5 target tabs, what would the "Form" and "Topic" values be? Are there rows 
   in the library for these tabs' tips? (Show the query to check.)

Report only with exact schema, code, file paths, and SQL. No edits.

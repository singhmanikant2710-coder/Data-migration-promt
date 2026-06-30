READ-ONLY. Do NOT edit any file. Report only.

On the Review Form CRM Findings tab (section=crm-findings-and-ratings), 
each finding row has a Component dropdown and a dependent Finding Code dropdown. 
Some rows render "Select..." (empty) for Component and/or Finding Code even though 
the saved values exist and are valid.

Trace the rendering logic and report ONLY (no edits):
1. Which component/file renders the CRM Findings rows and the two dropdowns?
2. How are the Component dropdown options sourced/loaded?
3. How are the Finding Code dropdown options sourced — are they filtered by the 
   selected Component (cascading/dependent)? Show that logic.
4. How is each row's SAVED Component and Finding Code value matched against the 
   dropdown options to set the selected value? 
5. Is there any timing/order issue where the Finding Code value is set before its 
   options (dependent on Component) are populated?
6. Could a value mismatch (whitespace, casing, type) cause the option match to fail?

Report findings only. List the exact file paths and code sections. No code changes.

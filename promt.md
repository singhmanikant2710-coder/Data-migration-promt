READ-ONLY first. Do NOT edit yet. Report only.

In the frontend, the string "06-Data Integrity" is incorrect and must become 
"06-Servicing Systems" to match the database.

Search the entire frontend codebase and report EVERY file and line where 
"06-Data Integrity" or "Data Integrity" appears (type definitions, constants, 
arrays, mappings, anywhere).

Report only. No edits.

Modify ONLY frontend/src/app/review/[ecif]/review-info/components/sections/hooks/useCrmFindings.ts

Replace every occurrence of "06-Data Integrity" with "06-Servicing Systems" 
in this file (both the CrmComponentId type and the COMPONENT_OPTIONS array, 
and ALLOWED_COMPONENTS if present here).

Do NOT change any other file. If another file also contains "06-Data Integrity", 
STOP and tell me — do not edit it.

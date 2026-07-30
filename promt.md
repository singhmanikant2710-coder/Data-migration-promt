-- PD Grade: kya Selection_id order = intended numerical grade order?
SELECT Selection_id, Selection
FROM dbo.[03_LIBRARY_09_Selections]
WHERE LTRIM(RTRIM(Tab)) = 'Scorecard' 
  AND LTRIM(RTRIM(Section)) = 'PD Grade'
ORDER BY Selection_id;

-- LGD Grade: same check
SELECT Selection_id, Selection
FROM dbo.[03_LIBRARY_09_Selections]
WHERE LTRIM(RTRIM(Tab)) = 'Scorecard' 
  AND LTRIM(RTRIM(Section)) = 'LGD Grade'
ORDER BY Selection_id;

-- Report Selections: expected order for #170
SELECT Selection_id, Selection
FROM dbo.[03_LIBRARY_09_Selections]
WHERE LTRIM(RTRIM(Tab)) = 'Reporting' 
  AND LTRIM(RTRIM(Section)) = 'Report Selections'
ORDER BY Selection_id;


READ-ONLY. Diagnostics only.

The Maintenance → Selections screen (frontend/src/app/maintenance/selections/page.tsx) 
shows a full "Values" list currently ordered by Tab, then Section, then 
Selection_id. 

Confirm: does the MAIN grid/list on that screen (the full multi-row table 
of all selections) come from GetSelectionsByTabAndSectionAsync, or from a 
DIFFERENT method (e.g. GetAllAsync)? 

I need to know whether changing GetSelectionsByTabAndSectionAsync's ORDER BY 
would affect that screen's Tab→Section→Selection_id ordering, or only the 
per-(tab,section) value dropdown within it.

Do not edit anything. Findings only.

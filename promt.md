READ-ONLY. Read once. Do not search broadly or re-read.

Find where the Sample "Type" dropdown options are defined (Samples screen, 
creating a new Sample). Two typos need fixing:
- "Continous" should be "Continuous" (missing "u")
- "Others" should be "Other" (not plural)

Search for the dropdown option strings. Likely in a Samples component or a 
constants/enum file. Show:
1. The file path and the exact code where these Type options are defined 
   (the array/enum/list of dropdown values).
2. The exact current strings ("Continous"/"Continuous" and "Others"/"Other") 
   so I can see the precise typos.
3. Whether these values are ALSO used elsewhere (e.g. saved to DB, compared in 
   logic) — so fixing the display text doesn't break a value comparison. 
   Specifically: is the dropdown VALUE (what's saved) different from the LABEL 
   (what's shown)? If they're the same string, changing it might affect saved 
   data or filters — flag that.

Do NOT edit. Show where the Type options are defined and whether the string is 
used as a stored value vs just a display label. Findings only.

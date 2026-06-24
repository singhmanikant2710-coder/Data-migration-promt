File to modify: backend SqlReviewStatusRepository.cs (ONLY this file)

ROOT CAUSE (confirmed): The review-status sample dropdown sends 
Samples.Sample_id (e.g. 136), but both the count-bucket query and the 
"Completed Draft Reviews" query filter Reviews.Sample_id = @sampleId, where 
Reviews.Sample_id stores the leading number from Sample_name (e.g. 357). So 
136 never matches 357 → "No records" for individual samples, while "All 
Samples" works (no filter).

FIX: Where the samples dropdown list is built in this repository, change each 
sample's Id so it equals the leading number parsed from Sample_name (the part 
before the first " - "), instead of Samples.Sample_id. Keep Name = full 
Sample_name unchanged.

Current code (approx):
.Select(s => new LookupItem { Id = s.Sample_id.ToString(), Name = s.Sample_name ... })

Change the Id to the parsed leading number:
- Take Sample_name, take the substring before the first " - " (or first "-"), 
  trim it, and use that as the Id (as a string).
- If parsing fails or there's no separator, fall back to s.Sample_id.ToString() 
  so nothing breaks.
- Do this in a null-safe way (Sample_name could theoretically be null — fall 
  back to Sample_id then).

Do NOT change the count-bucket query or the "Completed Draft Reviews" query — 
they already filter Reviews.Sample_id = @sampleId, which will now match because 
the dropdown sends the parsed number (357) that exists in Reviews.Sample_id.

Modify ONLY SqlReviewStatusRepository.cs, only the part that builds the samples 
dropdown list. Do NOT touch the frontend, the controller, the service, or any 
query logic. If anything else seems to need changing, STOP and ask me first.

After editing, paste back the changed Select/mapping code so I can verify.

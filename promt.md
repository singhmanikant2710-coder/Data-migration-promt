Modify ONLY this file:
backend/src/Casrr.Application/Services/ReviewService.cs

BUG FOUND: The frontend sends "findings" as a JSON OBJECT with numeric string keys 
({"0":{...},"1":{...},"2":{...}}), NOT as a JSON array. The current code only handles 
JsonValueKind.Array, so the loop is skipped and nothing saves.

FIX the CRM Findings block to handle BOTH shapes:

1. Remove the debug throw ("CRM_DBG: ...") I added.

2. Change the findings extraction so it iterates whether vFindings is an Array OR 
   an Object:
   - If vFindings.ValueKind == JsonValueKind.Array: iterate vFindings.EnumerateArray() 
     as before.
   - If vFindings.ValueKind == JsonValueKind.Object: iterate vFindings.EnumerateObject() 
     and use each property's .Value as the finding element.
   
   Build a single list of finding elements from either shape, then run the SAME 
   per-element parsing (component, findingCode, severity, comments, followUp) and 
   SaveCrmFindingsAsync call that already exists.

3. Keep the call to _repo.SaveCrmFindingsAsync(resolvedReviewId, findingRows, ct) 
   AFTER the loop (once, with the full list) — not inside the loop.

Restore a normal try/catch that logs (not swallows) but does not crash.

Modify ONLY ReviewService.cs.

Modify ONLY this file:
backend/src/Casrr.Application/Services/ReviewService.cs

In the CRM Findings block inside SaveAsync, remove any leftover test/real-error 
throws, then right BEFORE "var findingRows = new ..." add this one debug line:

   throw new Exception("CRM_DBG: kind=" + data.ValueKind + " | json=" + data.ToString());

Modify ONLY ReviewService.cs. Nothing else.

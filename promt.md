Modify ONLY this file:
backend/src/Casrr.Application/Services/ReviewService.cs

In the CRM Findings block inside SaveAsync, the catch is currently empty and 
swallows the real error:

    catch
    {
        // best-effort parse; ignore failures here
    }

TEMPORARILY change this catch so the real exception is NOT hidden. Replace it with:

    catch (Exception crmEx)
    {
        throw new Exception("CRM_FINDINGS_SAVE_FAILED: " + crmEx.ToString(), crmEx);
    }

This will surface the actual error in the save response/logs so we can see what 
is failing.

Modify ONLY ReviewService.cs. Do not change anything else.

Modify ONLY this file:
frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

Wire the trash icon to delete the finding from the DB, with confirmation.

Current handler:
  onClick={() => { deleteRow(row.id); }}

Change to:
1. Get reviewId — the page URL has ?reviewId=... (e.g. 12533). Read it via 
   useSearchParams (next/navigation) if this is a client component, or however 
   reviewId is already accessed here. Show me if reviewId is accessible.
2. On trash click:
   - If row.findingCode is empty/null (new unsaved row): just call deleteRow(row.id).
   - Else: window.confirm("Delete this finding?"). If confirmed:
       await deleteCrmFinding(reviewId, row.findingCode);
       then deleteRow(row.id);   // remove from UI on success
     On error, keep the row (do not call deleteRow).
3. Import deleteCrmFinding from "@/services/api/reviews".

Modify ONLY CrmFindingsAndRatingsSection.tsx. If reviewId is NOT accessible in this 
component and needs a prop/context from another file, STOP and tell me first.

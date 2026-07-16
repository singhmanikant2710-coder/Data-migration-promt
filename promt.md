Frontend only. Add a read-only "Distribution Parties" maintenance list page, mirroring the Loan Codes page pattern but WITHOUT any add/edit/delete (read-only). Single file per edit. Do not plan. Just apply.

The backend endpoint already exists: GET /api/v1/distribution-parties returns an array of { recipientName, recipientEmail, recipientRole }.

Create these files:

1) API client: frontend/src/services/api/distributionParties.ts
   Follow the style of frontend/src/services/api/loanCodes.ts.
   export interface DistributionParty {
     recipientName: string;
     recipientEmail: string;
     recipientRole: string;
   }
   export async function listDistributionParties(): Promise<DistributionParty[]> {
     return await get<DistributionParty[]>('/api/v1/distribution-parties');
   }

2) Page: frontend/src/app/maintenance/distribution-parties/page.tsx
   Copy the visual pattern from frontend/src/app/maintenance/loan-codes/page.tsx — same header (title "Distribution Parties" + subtitle), same dark-navy table header, same alternating rows, same skeleton loader while loading, same pagination bar with "Rows per page", same toast style for load errors, same "Filter rows" search box that filters client-side across all three columns.
   BUT read-only: NO Add button, NO edit/save/cancel, NO delete, NO delete-confirm modal, NO inline add row. Just display the rows.
   Columns: Recipient Name | Recipient Email | Recipient Role.
   Fetch via listDistributionParties() on mount.

Do not modify the sidebar in this step. Do not touch any other maintenance page.

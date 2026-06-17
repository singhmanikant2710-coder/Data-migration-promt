Does selecting a Sample/Review Name from the dropdown filter the table to only that sample's records, or does "All Finalized Reviews" remain the default view until cleared?
Should Borrower Name Search support partial match/autocomplete, or exact match only? Should it also support searching by eCIF# (Customer Number), not just name?
Can the Sample dropdown and Borrower Name search be used together as combined filters, or are they mutually exclusive?
Review Queue has a "My View / My Team / My Department" filter — it's missing on Review History. Is this intentional (all finalized reviews visible org-wide), or should it be added here too?
With 60+ records expected, is pagination needed or infinite scroll? What should the default page size be?
What should the default sort order be (e.g., Completed date, most recent first)? Should columns be sortable by clicking headers?
Does the "Completed" date represent when the review was finalized, or when it was distributed?
When clicking the Borrower Name link, should it open the review form in read-only/view mode (since it's finalized), or should editing still be allowed?
Does the document icon open the same linesheet print view (CAS Docs) as in Review Queue?
Should Bank PD / CAS PD values be editable here, or strictly locked since the review is finalized?
Is an Export to Excel/CSV feature needed for audit/reporting purposes?
Should there be a filter by Reviewer as well, or is Borrower Name search sufficient?
Does Review History data come from the same "Reviews" table/entity used by Review Queue (filtered by status = Finalized), or a separate read-only/reporting data store?
Where do fields like eCIF#, Exposure, Bank PD, and CAS PD originate — the same CRM/core banking integration used elsewhere, or Origenate pipeline-synced Dataverse data?
When a review moves to "Finalized" status, does it reflect in History in real time, or is there a batch sync/nightly refresh process?
Do finalized reviews remain permanently in the database, or is there an archive/purge policy after some retention period?
Will this screen query the production database directly, or a staging/reporting replica — important for performance given the search/filter/pagination load?

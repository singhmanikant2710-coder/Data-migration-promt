Confirmed in the live DB: dbo.[02_CORE_02_Reviews] HAS Portfolio_mgr_lead_email
(nvarchar). Query result:
  Portfolio_mgr_lead_name   nvarchar
  Portfolio_mgr_lead_email  nvarchar
discovery/backend-schema/columns.csv is stale; don't rely on it for this.

1. Read path: add Portfolio_mgr_lead_email to all three review-read SELECTs,
   the Row, and the CustomerInfoSection DTO as PortfolioManagerLeadEmail,
   using the same ReadNamedString / append-at-end pattern as the other four
   emails. Confirm the modal reads "portfolioManagerLeadEmail" for CC; fix
   the key only if it differs.
2. Save path: at the existing // DEVELOPER: marker in SaveCustomerInfoAsync
   (~SqlReviewRepository.cs:1300), persist Portfolio_mgr_lead_email when PML
   is updated, the same way ECO_email / SCO_email are persisted (the screen
   already sends portfolioManagerLeadEmail). Remove the outdated comment that
   says the column could not be confirmed.
3. Don't change RM/PM/ECO/SCO logic, the resolver, sample-load, or any other
   screen.

Report ADDED/REMOVED (file:line); on-screen behaviour: CC pre-fill with PML
set, and after changing PML + Save then reopening Email. NULL case: PML not
set (CC = SCO/ECO only, no stray ";"); PML cleared on save (email becomes
NULL, not a stale value). Build results. Do not commit.

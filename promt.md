Moving forward — enough SQL verification, let's finalize this feature.

Confirmed context: Recipient_role storing Employee ID in Distribution Parties
is intentional per Geoff/DB team direction. The mismatches found during testing
(format differences, and one stale legacy record in review 21592) are not
logic bugs — they're expected data artifacts we've already accounted for with
the NULL-fallback and normalized-name warning tripwire.

Final requirement before this is merge-ready:

1. Confirm (just point me to the exact line/method, one-line answer, no more
   SQL needed) that the RM/PM email resolver runs against the employee ID the
   user JUST SELECTED in the dropdown at save time — i.e. it queries
   Distribution Parties/Data Mart fresh at the moment of save, using the
   currently-selected ID. It must NOT re-derive the match from whatever
   name/ID is already sitting in the review record's stored fields (which
   could be stale/legacy, as we saw with review 21592).

   If it already works this way — confirm and move to step 2.
   If it currently re-derives from stored data instead — fix it now to use
   the freshly-selected ID from the dropdown at save time.

2. Once #1 is confirmed correct, this feature (RM/PM/PML/ECO/SCO dropdowns +
   search + Recipient_role removal + ID-based email resolution + normalized-
   name warning tripwire) is done. Merge it.

3. Remaining open item, separate from this merge: Portfolio Manager Lead email
   column — still needs the INFORMATION_SCHEMA.COLUMNS check
   (check-customer-info-people-columns.sql) run against CURRENT data to
   confirm whether that column exists yet. This does not block today's merge
   — Task 4 (Email Functionality) stays blocked on it, but everything else can
   ship now.

Don't wait on the Distribution Parties full-930-user repopulation to merge —
the code is source-agnostic (it queries whatever's in the table at save time),
so it'll pick up the refreshed data automatically once the DB team completes
that load. We'll do QA once you confirm the DB team has finished the
repopulation.

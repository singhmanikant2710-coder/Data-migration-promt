Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project (First Horizon Bank). Client requirement: "Update RM / PM / PML /
Credit Drop-Downs" on the Review Form's Customer Info page (Relationship
Overview section) — 5 fields: Relationship Manager, Portfolio Manager, Portfolio
Manager Lead, Executive Credit Officer, Senior Credit Officer.

IMPORTANT: Do not modify, remove, or refactor any existing working functionality
(the Reporting page's RM/PM search filters, other Review Form sections, or any
other dropdown) other than the specific fields described below.

Current state (confirm by inspecting the code before changing anything):
- Relationship Manager and Portfolio Manager already source from the Distribution
  Parties table and already display "EMPLOYEE_ID - LAST, FIRST" format (e.g.
  "48191 - LEO MUTCHLER") — this part is DONE, don't rebuild it.
- Portfolio Manager Lead, Executive Credit Officer, and Senior Credit Officer
  also source from the Distribution Parties table, but their query currently
  applies a filter on a Recipient_role column that restricts which rows show up.
- NONE of the 5 dropdowns currently have a search/filter-as-you-type box inside
  them — they are plain scrollable "Select..." dropdowns. Compare this to the
  Reporting page's Relationship Manager / Portfolio Manager filters, which DO have
  a working search-as-you-type box (type "john" → list filters live) — reuse
  whatever shared component/pattern powers that search, if one exists, rather than
  building a new search implementation from scratch.

Tasks:

1. Remove the Recipient_role filter condition on the Portfolio Manager Lead,
   Executive Credit Officer, and Senior Credit Officer dropdown queries, so they
   pull the full Distribution Parties list the same way RM/PM already do. Do not
   change the RM/PM queries.

2. Add search-as-you-type functionality inside all 5 dropdowns (Relationship
   Manager, Portfolio Manager, Portfolio Manager Lead, Executive Credit Officer,
   Senior Credit Officer) on the Review Form's Customer Info page. Reuse the same
   searchable-dropdown component/pattern already used on the Reporting page
   filters (check `SearchableSelect` or equivalent — this project already has UAT
   history of a "SearchableSelect flip-up positioning" fix, so a shared component
   likely already exists; reuse it, don't duplicate it).

3. Display format "EMPLOYEE_ID - LAST, FIRST" is already correct for RM/PM.
   Per the client spec, this ID-Name display format is only required for
   Relationship Manager and Portfolio Manager — leave Portfolio Manager Lead,
   Executive Credit Officer, and Senior Credit Officer showing name only (unless
   inspection shows they already show ID too, in which case leave that as-is —
   don't remove it).

4. Back-end storage on save:
   - When Relationship Manager and/or Portfolio Manager fields are updated, store
     the associated name, employee ID, AND email address in their respective
     back-end table fields.
   - When Portfolio Manager Lead, Executive Credit Officer, and/or Senior Credit
     Officer fields are updated, store the associated name AND email address
     (no employee ID required for these three) in their respective back-end
     table fields.
   Check the existing save/update logic for RM/PM first — if it already stores
   name+ID+email correctly, use it as the reference pattern for wiring up
   PML/ECO/SCO's name+email storage; don't rebuild working RM/PM save logic.

Acceptance criteria:
- All 5 dropdowns on the Customer Info page source from Distribution Parties with
  no Recipient_role restriction on PML/ECO/SCO.
- All 5 dropdowns have working search-as-you-type, matching the Reporting page's
  existing search UX.
- RM/PM continue to display "EMPLOYEE_ID - LAST, FIRST"; PML/ECO/SCO display
  format is unchanged unless it already included ID.
- Saving the form correctly persists name+employeeId+email for RM/PM changes, and
  name+email for PML/ECO/SCO changes, into their respective back-end fields.
- Reporting page's existing RM/PM search filters and all other Review Form
  sections remain unaffected.

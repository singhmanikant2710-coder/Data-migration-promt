Context: .NET 8 Clean Architecture backend + Next.js/React/TypeScript frontend,
CASRR project. Client has clarified a requirement: RM/PM names stored in
review records must be consistent across all reviews (currently a name might
be stored as "JOHN DOE" on one review and "DOE, JOHN" on another, depending on
source), because this breaks downstream reporting filters by RM/PM name.

IMPORTANT: Only touch RM/PM name-storage logic on save. Do not change the
dropdown's search/selection UX, the employee-ID-based email resolution, or
PML/ECO/SCO logic already completed.

Current behavior (confirm by inspecting the code first): when a user selects
an RM/PM from the Customer Info dropdown (sourced from Data Mart Trial) and
saves, the review record stores the Data Mart Trial display name format
(FIRST MIDDLE LAST), while Distribution Parties stores the canonical format
(LAST, FIRST MIDDLE). This is what's causing the inconsistency the client
flagged.

Task: When saving Relationship_mgr_name / Portfolio_mgr_name, after resolving
the employee ID's matching row in Distribution Parties (which we already do
for email), also use THAT row's Recipient_name as the name to store — not the
Data Mart Trial dropdown's display label. This makes every review's stored RM/PM
name consistent with the Distribution Parties canonical format, regardless of
which screen/process wrote it.

Fallback: if no matching Distribution Parties row is found for the selected
employee ID (as we've seen happens for ~79% of RM records), fall back to
storing the Data Mart Trial display name as before — don't leave the name
blank just because the email/canonical-name lookup failed.

Acceptance criteria:
- Saving an RM/PM whose employee ID has a Distribution Parties match stores
  the Distribution Parties canonical name (LAST, FIRST format).
- Saving an RM/PM with no Distribution Parties match still stores the
  dropdown's name as a fallback (no blank names introduced).
- Email and employee ID storage logic already implemented is unchanged.

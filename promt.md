Context: CASRR. Spec items 6-7: the Customer Info RELATIONSHIP MANAGER and
PORTFOLIO MANAGER dropdowns must list their options from
[03_LIBRARY_10_Distribution Parties] (Recipient_name), not from
[01_DATA_01_Data Mart Trial]. Selecting a person must save that person's
number (Recipient_role), name (Recipient_name) and email (Recipient_email)
from Distribution Parties.

Current state: options come from GetDataMartRelationshipManagersAsync /
GetDataMartPortfolioManagersAsync (Data Mart, padded via PadEmployeeIdSql),
consumed by CustomerInfoSection.tsx:179-180. On save, the ID resolver
(ResolveDistributionPartyByEmployeeIdAsync) already fetches the canonical
name + email from Distribution Parties.

Requirements (generic fix, don't break other screens):
1. RM and PM dropdown options come from Distribution Parties. Label format
   stays "ID - NAME", e.g. "17436 - WAGNER, JOHN C". Only include rows with
   a non-blank, non-all-zero Recipient_role and a non-blank Recipient_name.
   Order by name. Search-as-you-type must keep working.
2. Save path: keep the existing SplitNumberName + ID resolver flow, so
   number/name/email all come from Distribution Parties. Don't create a
   second, competing save path.
3. Pre-selection for existing reviews: match the stored value to an option
   by Employee ID (numeric compare, i.e. '08784' = '8784'), not by the full
   label text. A review whose stored name is in the old Data Mart format
   ("JOHN C WAGNER II") but whose ID exists in Distribution Parties must
   pre-select the Distribution Parties option, with NO duplicate synthetic
   entry. If the stored ID is not in Distribution Parties, keep the existing
   ensureIncludesSelected behaviour (show it as-is).
4. NULL / all-zero stored ID: the field shows empty or name-only, exactly as
   in the last change.
5. Do NOT change: the Reports page RM/PM filters, the sample-load NULL-on-miss
   INSERT, PML/ECO/SCO dropdowns, or the Distribution Parties maintenance
   screen. If the Data Mart lookup endpoints become unused, report it, but
   don't delete them.

Report:
- ADDED and REMOVED lines (file:line)
- On-screen change for Customer Info RM/PM: option list source, label,
  pre-selection for (a) a matched ID, (b) an old Data Mart-format name with
  a matched ID, (c) an ID not in Distribution Parties, (d) NULL / all-zero
- What gets saved to DB in each case
- Backend + frontend build results
Do not commit.

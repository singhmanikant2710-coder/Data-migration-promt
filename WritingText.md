UAT #247 (part 2) – "CCL Managed" Yes/No at customer level, synced with all accounts (same as Special Assets Managed)

CONTEXT
- Work on the current branch (feature/uat-247-special-assets), which already contains the approved Special Assets Managed build (#247): customer-level dropdown, two-way sync with all accounts in one transaction after the #232 version claim, #233 only-if-changed rule, 1A conflict rejection, hints, 6B new-account default, NULL shown as blank, three reports switched to review level. Geoff approved it.
- Geoff's answers for CCL:
  1. New "CCL Managed" Yes/No dropdown shows dbo.[02_CORE_02_Reviews].CCL and stays in sync with Accounts CCL EXACTLY like Special Assets Managed (both directions).
  2. Placement in the Customer Profile section (Customer Info tab): move "FHN Portfolio Segment" to directly below "FHN Portfolio" (pushing "FHN NAICS Industry" down), and put "CCL Managed" below "Customer Size", where FHN Portfolio Segment sits today. If that layout is not feasible, place CCL Managed under FHN NAICS Industry – report which you did.
  3. New accounts (Add Account) default their CCL to the customer-level CCL value (same as 6B).
  4. ALL reports that filter on CCL must use the customer-level Reviews.CCL value.
  5. Existing mismatched data stays as is until a reviewer changes CCL (no automatic clean-up). Geoff mainly cares about 2026 onwards; Ashok is correcting Production CCL data.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Exact columns, types, nullability and defaults of CCL on Reviews and Accounts (give me the read-only SQL, same style as for Special_assets), distinct values incl. NULL, and reviews whose accounts disagree.
- Every place CCL is written today (sample load, Account Details checkbox, Add Account) and read (Review Form DTOs, PDFs, exports) with file:line. Is Reviews.CCL loaded into the Review Form today?
- Every report/filter that uses CCL (file:line, table/column, NULL handling), split into: already review level vs account level.
- How the #247 Special Assets code is structured (file:line) and how to reuse it for CCL WITHOUT copying: e.g. one generic "customer-level flag synced to accounts" path parameterised by field (Special_assets, CCL).
- Customer Profile layout (CustomerInfoSection.tsx) and how the requested placement fits the grid.

PHASE 2 – PLAN (smallest generic change)
- Reuse/generalise the #247 logic so Special Assets and CCL share one implementation. Special Assets behaviour must stay EXACTLY as tested (same messages, rules, conflict check, hints, defaults).
- Both flags can change in the same save; each is checked and synced independently (a Special Assets conflict must not block a valid CCL change, and the reverse – or report if one error message for both is simpler).
- Reports: switch every account-level CCL filter to the review-level form with the same NULL handling as Special Assets; leave review-level ones as they are. Give a read-only before/after count SQL for Geoff.
- Do NOT decide silently – list anything ambiguous as a decision with a recommendation.
- No schema change. SQL only as idempotent scripts/sql/ if truly needed – ask first.

PHASE 3 – IMPLEMENT (only after I confirm the plan)

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- ADDED (file:line), REMOVED/CHANGED (file:line).
- On-screen before/after: new Customer Profile layout; CCL customer No→Yes / Yes→No; account checkbox No→Yes / Yes→No; conflict in one save; save without touching CCL; Add Account default; both Special Assets and CCL changed in one save; Special Assets regression check (all #247 cases).
- Read-only verification SQL for Reviews + Accounts CCL per Review_id.
- Reports: list of reports switched and before/after counts.
- dotnet build, test projects, npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.

Thanks Geoff – glad the Special Assets build looks good. I'll build CCL Managed the same way, with your preferred layout in Customer Profile, the new-account default, and customer-level CCL filtering in all reports. I'll release Special Assets and CCL to QA together once CCL is tested.

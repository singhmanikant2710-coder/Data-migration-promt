UAT #247 – Review Form: "Special Assets Managed" Yes/No at customer level, synced with all accounts

REQUIREMENT (Geoff, Issue 247)
1. Relationship Overview section (Review Info): add a Yes/No dropdown labelled "Special Assets Managed", next to Unit (under it, see screenshot). It shows dbo.[02_CORE_02_Reviews].Special_assets (Yes/No | True/False) as Yes or No. Reviewers can change Yes↔No.
2. Changing it updates dbo.[02_CORE_04_Accounts].Special_assets to the same value for ALL accounts of the current Review_id.
3. Changing the Special Assets checkbox for one account in Transactions tab > Account Details updates Reviews.Special_assets AND Special_assets on ALL other accounts of the same Review_id to that value.
Business rule: if Special Assets manages a customer, it manages ALL loans of that customer. Differing account values are a data-integrity issue CAS raises with the bank – CASRR should not silently "fix" them on load.
4. Reports: check how the Special Assets filter (Reports home) works. If it uses the account-level field, redirect it to the customer-level Reviews.Special_assets.

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- Exact column names and data types of Special_assets on Reviews and on Accounts (bit / nvarchar 'Yes'/'No' / 'True'/'False'?), nullability, and the actual distinct values present in Dev (give me a read-only SQL to check, incl. NULLs and reviews whose accounts disagree).
- Where Special_assets is written today with file:line: sample load, Data Mart, Account Details save, any other path. Is Reviews.Special_assets loaded into the Review Form DTO at all?
- Relationship Overview: component/file:line, how its fields load and save (main toolbar Save payload, #233 send-only-edited rules).
- Transactions > Account Details: file:line of the Special Assets checkbox, how account edits are staged and saved, and how the account list refreshes after save.
- How the save is processed on the server (ReviewService / repository), transactions used, and how the #232 version check/bump (SSMA_TimeStamp, TryClaimReviewVersionAsync/TouchReviewVersionAsync) applies to account-only saves.
- Reports: every report/filter that uses Special_assets (file:line, which table/column, how True/False/NULL is matched). List all hits.
- Every other reader of Accounts.Special_assets / Reviews.Special_assets (PDFs, exports, Review Queue export #237, Core Export) that might be affected.

PHASE 2 – PLAN (smallest generic change)
- One server-side rule used by both paths: when Special_assets is changed for a review (customer field or any account), set Reviews.Special_assets and Special_assets on all accounts of that Review_id to the new value, in ONE transaction together with the rest of the save, after the #232 version check, with the version bumped and the new token returned once.
- Only when the user actually changed the value this session (#233). Untouched NULL/mismatched values must stay exactly as stored. Opening/saving a review must never normalise data by itself.
- Store values in the existing format of each column (no format change). Display: Yes / No; NULL shows as blank "Select..." (not No).
- After save, the Transactions tab and Relationship Overview must both show the synced value without a manual reload; no stale staged account edit may overwrite it.
- Reports filter: point it to Reviews.Special_assets only if it currently uses Accounts; keep the filter UI and other filters unchanged.
- Locked reviews: field read-only like the other Relationship Overview fields.
- Do NOT decide silently – list options with a recommendation:
  1. In ONE save the user changes both the customer field and an account checkbox to different values – which wins (or block with a message)?
  2. Should there be a visible hint/confirmation that the change applies to all N accounts?
  3. Reviews.Special_assets NULL: show blank and allow choosing, or derive a display value from the accounts?
  4. Reports filter change: does it change any existing report results today? Show counts before/after with a read-only SQL so Geoff can be told.
- No schema change. SQL only as idempotent scripts/sql/ if truly needed – ask first.

PHASE 3 – IMPLEMENT (only after I answer the Phase 2 decisions)
- Touch only the needed code. #212, #232, #233, #235, #239/#240, #249, #254 behaviour stays unchanged.

PHASE 4 – VERIFY + REPORT (in chat, not in a file)
- ADDED (file:line), REMOVED/CHANGED (file:line).
- On-screen before/after: customer No→Yes, Yes→No; account checkbox No→Yes, Yes→No; review with 0 accounts; review with 1 and many accounts; save other fields without touching Special Assets (nothing changes); locked review.
- Read-only SQL to verify Reviews + all Accounts values for a Review_id after each case.
- NULL/edge cases: NULL on Reviews and/or some accounts; accounts already disagreeing before the change; save rejected by #212/#249/#232 (nothing synced); network error.
- Reports: filter result before/after.
- Confirm other screens/readers are unaffected.
- dotnet build, npx tsc --noEmit, npm run build results.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.

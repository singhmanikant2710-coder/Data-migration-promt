# BCAT — Fiscal Year Bug Investigation — Session Summary
*(Context handoff document — use this to continue in a new chat)*

---

## 1. Background

Started as a single UAT bug (Athens Paper Company Inc — Black Book save crashed with a PK violation). Investigating it led to discovering a shared root-cause defect in `SqlMainRepository.UpsertRowWithConnectionAsync` affecting every customer whose fiscal year doesn't start in January. Ended up finding and fixing 5 related bugs, then a 6th open thread (a genuine legacy data anomaly), and a 7th brand-new feature request from John.

---

## 2. Bugs Found & Fixed — ALL COMMITTED

All five below are fixed, verified against live data, and committed (`SqlMainRepository.cs`, `SqlCovenantRepository.cs`, `MainController.cs`).

1. **PK violation on edit.** The existence-probe and UPDATE WHERE clause matched on `(intFiscalYear=@fy AND intFiscalMonth=@fm) OR strMonthKey=@mk`, where `(fy,fm)` were calendar-split from the month key with no fiscal-start awareness. For non-January-start customers this could match a *different* real row, and the UPDATE also rewrote `strMonthKey` (the PK) → duplicate-key crash.
   **Fix:** probe/UPDATE narrowed to `(strCustomerName, strMonthKey)` only. PK is never rewritten.

2. **Fiscal month/year drift on new months.** New rows derived `intFiscalMonth` as `prevRow.intFiscalMonth + 1` (position-based), not from the calendar — so any gap or pre-existing corruption propagated forward silently.
   **Fix — validated formula:**
   ```
   fy = (start == 1) ? y : (m >= start ? y + 1 : y)
   fm = ((m - start + 12) % 12) + 1
   ```
   Validated against **legacy MS Access directly** for 5 customers across 4 different start-months: Athens Paper (start=10), Imperial Trading (start=6), Charter Pipe (start=1), 1st Franklin (start=1), Colonial Auto Finance (start=5). All matched exactly.

3. **Covenant edits wrote to the wrong table.** Editing "Min Tangible Net Worth" (and other covenant fields) wrote to `tblMain`'s denormalized slot columns (`dblCovenantActual1..6`), but the display/read path pulls from `tblMainCovenants.strCovenantActual` — a different table. Edits appeared to save but never showed up.
   **Fix:** added `WriteCovenantActualsToCanonicalTableAsync`, writes to the canonical table in the same transaction. **Verified end-to-end** on Nationwide Specialty Finance: edited value 43469196 → 44469197, confirmed landed correctly in `strCovenantActual`.

4. **Wrong elapsed-fiscal-days formula.** Code used `DateDiff('d', datFiscalYearStart, firstOfMonth) + 30`, which doesn't match the legacy spec at all. The actual BCAT spec (from the original formula reference) is:
   ```
   intElapsedFiscalDays = intFiscalMonth * 30
   ```
   Verified **18/18 exact match** against real migrated Athens data, and separately against Charter Pipe (start=1). DateDiff matched 0/18 — even with a "correct" `datFiscalYearStart`, it can't reproduce legacy's idealized 30-day-month values.
   **Fix:** `UpdateElapsedFiscalDaysForRowAsync` now computes `intFiscalMonth * 30`.

5. **`datFiscalYearStart` never updated on edit.** It was only set in the INSERT branch — editing an *existing* row corrected `intFiscalYear`/`intFiscalMonth` but left `datFiscalYearStart` permanently stale (confirmed live on Nationwide 202601: showed `2025-06-01` instead of `2026-06-01` after an edit).
   **Fix:** hoisted the computation above the branch, added to the UPDATE SET list too.

**Also fixed:** a same-day regression — `MainController.cs`'s save-order change caused `SqlCovenantRepository.SeedFromLatestAsync` to insert a minimal, badly-formed `tblMain` row *before* the real upsert ran, leaving `datFiscalYearStart` NULL on new months. Reverted `MainController.cs` to HEAD; removed the redundant `tblMain` insert from `SeedFromLatestAsync` (no FK dependency — confirmed via schema check, both `tblMain`/`tblMainCovenants` report 0 foreign keys).

---

## 3. Validation Performed

- **9 distinct fiscal-start-months** exist in the customer base (1,2,4,5,6,7,9,10,11). Formula validated directly via legacy Access for 4 of these; math-consistent across all 9 via full-database scan.
- **Full population scan** (521 total customers, 212 with checkable `tblMain` data):
  - 179 calendar-year (Jan start) — convention question doesn't apply.
  - 23 fully match the formula, zero mismatches.
  - 6 "mixed/partial" — **known, separate** corruption from the old `prevRow+1` bug (B W I Companies, Athens Paper, Hankins, John W Stone Oil Distributors, Frez-N-Stor, Van Zyverden). Self-heals when a row is individually resaved; a few of these were confirmed to self-heal live. No formal backfill has been scheduled — **still open, low priority**.
  - **4 customers** — 100% consistent OPPOSITE ("starting-year") convention across their *entire* history. See §4.

---

## 4. The 4-Customer Fiscal Convention Anomaly — RESOLVED (root cause), pending execution

**Customers:** Bankers Healthcare Group LLC, Keystone Private Income Fund, Nationwide Specialty Finance Inc, Vermeer Mountain West Inc.

Their fiscal year is labeled by the year it **starts** in (e.g. June 2025–May 2026 = "FY2025"), opposite of every other non-calendar customer (which labels by the year it **ends** in).

**5 hypotheses tested, all ruled out with real data — this was not guesswork:**
1. Formula wrong? No — verified correct against 5 other customers via legacy Access.
2. Flag on `tblCustomer`? No — checked all 200+ columns, nothing informative.
3. Industry-based? No — checked all 22 DirectAuto customers; a same-industry customer (Colonial Auto Finance) follows the *normal* convention.
4. Start-month-based? No — Imperial Trading and Nationwide are both June-start; only one is an exception.
5. Lender/syndication structure? No — pulled lender counts from `tblCustomerAdditional`; syndicated customers split 10-normal/2-exception, no correlation.

**Root cause confirmed:** checked these 4 customers' **very first-ever row** directly in legacy Access — already showed the "opposite" convention from day one. Not a migration bug. A genuine, old, legacy-side data-entry decision that was never questioned because legacy never compares one customer's labeling to another's (every screen/report is single-customer scoped).

**John's response:** *"Ok, we will fix the data in legacy. Please make sure the code is consistent, including if a user later changes the fiscal year in the customer edit screen. All downstream data should align to this consistent programmatic model."*

**Open question sent to John, awaiting reply:** does "fix the data in legacy" mean Access-only, or will he also correct these 4 customers' historical rows in SQL Server dev/prod? If Access-only, we need our own backfill on the SQL side.

---

## 5. New Work In Progress: Fiscal-Start-Change Cascade

**Why:** John's message above implies a new requirement — if a user changes a customer's fiscal-start-month via the Customer Edit screen, all downstream `tblMain` data must realign.

**Investigated (read-only) — confirmed:**
- The UI flow exists (`/customer/edit`, "Fiscal Year Start (Month)" dropdown) and is **not gated on existing history**.
- **Legacy Access had this field LOCKED after initial setup** ("Disable selection once initially setup" — VBA comment). That lock was dropped during migration, seemingly unintentionally. In legacy, this scenario literally could never occur.
- Currently: changing the field updates only `tblCustomer`. **Nothing cascades.** Existing rows keep stale `intFiscalYear`/`intFiscalMonth`/`datFiscalYearStart`/`intElapsedFiscalDays` (and everything derived from them) until each month is individually resaved.
- Two server + client caches also don't get invalidated on change (600s TTL server-side; unbounded client-side `window` cache).

**Presented two options to John: (a) restore the legacy lock [low-risk], or (b) build a full cascade [bigger build]. John explicitly chose (b), full cascade, and named `intElapsedFiscalDays` as an example field that must be included.**

**Windsurf has produced a design + diff.** Key points:
- **3-phase cascade required** (Windsurf caught an ordering bug in my original scope): Phase A (per-row fiscal fields), Phase B (per-distinct-fiscal-year YTD/TTM aggregates — re-partitioning shifts which months belong to which year), Phase C (per-row values that *consume* the aggregates, e.g. AR turn days, inventory turn). Running turns before aggregates would produce wrong numbers.
- Added 2 extra helpers beyond original scope (`UpdatePriorMonthValuesForRowAsync`, `UpdateCalculatedColumnsForRowAsync`) — agreed these are needed for correctness (month-boundary shifts affect prior-month joins).
- **Bug found in the diff, NOT yet fixed:** `SqlCustomerRepository`'s new "read prior fiscal start" code casts `ExecuteScalarAsync()`'s result directly to `(long?)`. `intFiscalYearMonthStart` is `smallint` → this **will throw `InvalidCastException`** at runtime. Must use the existing `ToInt()` helper (already used elsewhere in the same class) instead of the raw cast.
- **5 design decisions Windsurf asked about — recommended answers sent back:**
  1. Include the 2 extra helpers? → **Yes.**
  2. Transaction strategy (4 options offered)? → **Option 1 (no wrapping transaction, matches existing save-path behavior) + Option 3 (batch Phase A into fewer round-trips).** Rejected: full-transaction (lock contention risk) and background-job (overkill unless customers exceed ~150 months).
  3. Non-atomic `tblCustomer`/`tblMain` gap (cascade could fail after profile already saved)? → **Windsurf's option (b): make the cascade idempotent + add a separate admin "recompute now" endpoint** as the recovery path.
  4. Ship the frontend cache-invalidator now, or defer (current usage is presence-check-only, so currently low-risk)? → **Ship it now** — don't leave a latent bug for later.
  5. OK to change `ICustomerRepository.UpdateCustomerProfileAsync`'s signature (to return whether fiscal-start actually changed)? → **Yes** — the alternative (extra read-then-write) has a worse race condition.

**STATUS: waiting for Windsurf's corrected diff** (bug fixed + 5 decisions incorporated). **Nothing from this feature has been reviewed, approved, or applied yet.**

**Also flagged, not yet acted on:** `Bcat.Api.Tests` currently doesn't compile (56 pre-existing errors, unrelated to any of this work) — no controller-level test can be added until that's repaired separately.

---

## 6. Parked, NOT STARTED: "Min Tangible Net Worth" display inconsistency

Separate from the write-path bug fixed in §2.3 (which **is** fixed). This is about the **display/read** side, and it's a much bigger, still-unscoped architecture problem — deliberately not touched today.

**What's confirmed:**
- Backend has **4 duplicate copies** of the threshold-vs-actual preference logic (`MapMetricPoint` ×2 — SQL and Access backends — and `TryMergeCovenantsIntoSeries` ×2). They don't fully agree with each other (e.g. `MaxSeniorDebtTNW`'s own code comment says "prefer ACTUAL" but it's coded as threshold-preferring).
- Affects **7 covenant keys**, not just Min TNW: `MinProfitBeforeTaxes`, `MinFixedChargeCoverage`, `MaxDilutionPercent`, `MaxSeniorDebtTNW`, `MinInterestCoverage`, `MaxCARatio`.
- Frontend has **8 locations** (view/edit/report pages + `MonthSummaryTable`/`monthSummaryRegistry`) with **contradicting preferences** — the `view` page prefers threshold-first, `edit`/`report` prefer actual-first.
- A **third, independent mechanism**: `MonthSummaryTable`'s `computeMinTnwForRowLocal` computes a *live quotient* (Adjusted Liabilities ÷ Max Adjusted Debt/TNW Limit) and overrides everything in the "Summary (Top Strip)" render — this is why a correct DB value ($26,207) displayed as a stale, wrongly-formatted "26207.22%" there while the Monthly Summary grid correctly showed $26,207. (This grid cell is technically non-editable — `allowInlineEditing = false` — the real edit surface is a separate "covenant tile" component, confirmed working correctly.)

**Scope for whenever this is picked up:** verify each of the 7 keys against legacy Access individually, consolidate the 4 backend copies into one source of truth, consolidate the 8 frontend locations, decide the fate of the computed-quotient special case. **No work has started on this.**

---

## 7. How We Work With Windsurf — Reference for Continuing

Two prompt types, used consistently all day:

**READ-ONLY / investigation prompts** — used to diagnose *before* any code change:
- Always open with `READ-ONLY — do not propose or apply any fix.`
- Ask for exact code **quoted verbatim** with file/line references — never accept a paraphrase.
- End with `Report only. Do not propose or apply a fix yet.`
- Purpose: stop Windsurf from guessing/inferring. Several times today Windsurf initially mislabeled an *inferred* value as "confirmed" — always caught by asking "where did this actually come from — show me the real query/output."

**FIX / diff prompts** — used only once root cause is independently confirmed (via read-only investigation **and** our own SQL verification):
- Always say `Show diff only, do not apply.`
- **Never approve on Windsurf's confidence alone.** Manually re-derive the math/formula against real data before saying "approved" (this is how the fiscal-year formula sign error, the `datFiscalYearStart` UPDATE-branch gap, and the `ExecuteScalarAsync` cast bug were all caught — none of them were things Windsurf flagged itself first).
- Only after genuinely satisfied, send an explicit `Approved. Apply now.` — that is the only trigger that makes Windsurf actually modify files.
- After applying: **always** independently re-verify via a direct SQL query or a live UI edit + SQL check. Never trust "should work now."

**When Windsurf's output appears in a new tab:** read it fully, treat any "confirmed" claim as a hypothesis until you've seen the actual underlying query/data yourself, and don't move to the next step until that's settled. This discipline is what caught every real bug today — don't skip it even when Windsurf sounds certain.

---

## 8. Open Blockers / Next Steps

1. **Waiting on John:** scope of the legacy data-fix (Access-only vs. also SQL Server dev/prod) for the 4 exception customers.
2. **Waiting on Windsurf:** corrected fiscal-start-change cascade diff (bug fix + 5 decisions incorporated). Not yet reviewed or applied.
3. **Not started:** Min Tangible Net Worth / threshold-vs-actual display consolidation (§6) — needs its own dedicated investigation session.
4. **Not scheduled:** formal backfill for the 6 "mixed/partial" corrupted customers (§3) — currently relying on self-heal-on-resave, no proactive plan.
5. **Separate ticket, not addressed:** `Bcat.Api.Tests` doesn't compile (pre-existing, unrelated).

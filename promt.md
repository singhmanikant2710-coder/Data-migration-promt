Task: Backend only — fix the Review Status bucket counts. Read-only diagnostics first: open SqlReviewStatusRepository.cs, find the method computing the status square counts (GetStatusPageAsync or its helper), and report the current logic before editing. Single file. Use LIVE DB, ignore columns.csv. Do NOT change the sample dropdown, the grid, the Bucket filter, or pagination.

PROBLEM: The counts are currently computed as MUTUALLY EXCLUSIVE buckets (a CASE/if-else-if priority chain, or a GROUP BY on a derived status). Proof: they sum exactly to the total (109+16+20+118+0+1 = 264). This is WRONG.

The client's spec is intentionally OVERLAPPING — a review that is Approved still has Start_date NOT NULL, so it must be counted in BOTH "In Progress" AND "Approved". Buckets are independent flags, not stages.

FIX: Replace the count logic with seven INDEPENDENT counts over dbo.[02_CORE_02_Reviews], evaluated in the same scope the page already uses (join to dbo.[02_CORE_01_Samples] on Sample_id where Closed = 0 when "Select All", or filtered to the selected Sample_id). No CASE priority chain, no else-if:

  BorrowersSampled  = COUNT(*)
  UnopenedCancelled = COUNT WHERE [Start_date] IS NULL OR [Cancelled] = 1
  InProgress        = COUNT WHERE [Start_date] IS NOT NULL
  DraftCompleted    = COUNT WHERE [Completed_date] IS NOT NULL
  Approved          = COUNT WHERE [Review_approval_date] IS NOT NULL
  Distributed       = COUNT WHERE [Review_distributed_date] IS NOT NULL
  Finalized         = COUNT WHERE [Review_finalized_date] IS NOT NULL

They will overlap and will NOT sum to the total. That is expected and correct — do not "correct" it.

VERIFIED EXPECTED VALUES (already run against the live DB — the API must return exactly these):
  Select All (all reviews joined to samples where Closed = 0):
    264 / 112 / 158 / 136 / 118 / 0 / 116
  Single sample, Sample_id = 311:
    21 / 10 / 12 / 11 / 11 / 0 / 11

IMPORTANT — the Bucket dropdown filter must stay consistent with these counts: selecting a bucket must return exactly the rows matching that same independent condition (e.g. Bucket = "In Progress" returns all 158 rows where Start_date IS NOT NULL, including the ones that are also Approved). If the grid filter currently uses a derived single-status value, change it to use the same independent predicate. Report if the grid and the counts share a code path.

Answers to your 3 decisions for #244. Still NO implementation – wait for my Q1–Q5 and N1–N4 results.

1. Backdated Closed_Date: APPROVED – add the third guard. Exclude a sample if any review has Review_finalized_date or Cancelled_date >= cutoff (date-only). Reason: "Closed date is earlier than review activity – verify the closed date". Include it in the Excluded list and in the execute re-check.

2. Legacy closed samples with NULL Closed_Date: keep them EXCLUDED ("No closed date recorded – contact DBA"). No backfill in this item. Decision on a backfill comes after I see N3; if approved it will be a separate DBA script with business sign-off.

3. Existing server gaps (Continuous close check, Closed_Date editable/backdatable by any active user, reviews loadable into a closed sample): OUT OF SCOPE for #244. Do not touch SampleController, SqlSampleRepository or SampleLoad code. I will raise these separately.

Also:
- Zero-review closed samples being eligible under the new rule: accepted.
- Confirm from Q2 that Reviews.[Cancelled_date], Samples.[Sample_type] and Samples.[Sample_year] exist; if Cancelled_date does not exist, tell me and propose the alternative for guard 1.
- Your note about ISNULL(Cancelled,1) is incorrect – the previous Q6 used ISNULL([Cancelled],1)=1 in reviews_unfinalized_cancelled. No action needed, the new queries are consistent.
- The untracked devin-output/ folder must never be committed; I will delete it.

Reply in chat only. Do not create files. Do not commit or push.

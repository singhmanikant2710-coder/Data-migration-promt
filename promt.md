Thanks for the detailed response, Geoff — answering each point:

Re: cross-referencing — honestly, we worked primarily off the emailed specs for
the review form's RM/PM/PML/ECO/SCO dropdown work (items 6-7 in your Excel tab),
and had not yet implemented items 4-5 (the sample-loading append process that
populates Name/Number/Email from Distribution Parties at load time). We'll pick
that up now that you've flagged it — see below.

1. Confirmed, thank you — Portfolio_mgr_lead_email exists in
   02_CORE_02_Reviews. This unblocks the Email Functionality task; we'll
   finish that now.

2. This makes sense and we'll implement it — not too difficult at this stage.
   Two parts to it: (a) the sample-loading batch process (items 4-5) that
   populates RM/PM Name/Number/Email from Distribution Parties when a sample
   loads, and (b) making sure the review form's manual RM/PM save also stores
   the Distribution Parties canonical name format (not the dropdown's raw
   display name), so history stays consistent whether a record comes from
   batch loading or manual edit. We'll have this ready for your review shortly.

3. Thank you — glad the DP Maintenance layout works for you.

4. Good catch — investigating now. Our RM/PM matching already converts both
   sides to integers before comparing, so numeric join correctness (30 vs
   00030) isn't affected either way. But we'll check whether the leading zeros
   are actually missing from the underlying SQL data or just from the DP
   Maintenance screen's display, and fix accordingly. Will confirm shortly.

Looking forward to the notes on Checklist Questionnaire, CRM Summary for
Management, CRM Findings for Management, and CRM Findings & Observations —
happy to take those on once we close out the items above.

Thanks,
Manikant

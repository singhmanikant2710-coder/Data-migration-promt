Hi Geoff,

Thanks for re-sharing those designs from the 6.3c .mdb file — that's helpful. I dug into the four specifically and want to give you an accurate status (and correct one item from my earlier note):

1) Unsatisfactory Transactional Ratings — NOT built as a standalone report. Correcting my earlier message: what exists today is an "Unsatisfactory CRM Ratings" section inside the CRM Summary report, but there is no separate Unsatisfactory Transactional Ratings report wired to the Reports dropdown. This one still needs to be built. (Apologies — I initially listed it as built based on the dropdown entry; on inspection the standalone report isn't there.)

2) Non-Compliant Covenants (aka Covenant Violations) — Built. Both the Non-Compliant Covenants and Covenant Violations components are implemented and wired to run. One note: the Covenant Violations rendering sits behind a feature flag, so we may just need to enable/verify that flag. Your design will help us confirm the layout and columns match what you expect.

3) CRM Summary for Management / Review Summary (Findings Only) — Built. The Management Summary report exists and is wired, and it includes a "Findings Only" section that is currently behind a feature flag. So the capability is there; we'd align it to your 10_Review Summary design and enable the section.

4) CRM Findings for Management (Findings Only) — Not built as a distinct report. Today only the standard CRM Findings and Observations report exists. A management "findings-only" view currently lives only as an optional section within the Management Summary, not as its own report. This one would need to be built (or defined as a filtered variant of the existing CRM Findings report, per the Findings-only approach we discussed).

Net: #2 and #3 are built (may need feature-flag enablement + design alignment). #1 and #4 still need building. Since these are net-new builds rather than fixes, I'll raise them with Brijesh/the team to scope and sequence — your designs will be the reference. Will follow up with a plan.

Thanks,
Manikant

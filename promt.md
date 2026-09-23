Yes, please go ahead and check with Ashok and John on what's involved in
changing Relationship_mgr_number and Portfolio_mgr_number from Number to Text.

To be clear on our end: this change is purely for display/reporting fidelity
(so IDs like 00030 don't lose their leading zeros) — it has zero impact on
our RM/PM matching logic either way, since we already convert both sides to
integers when joining. So there's no urgency from a functionality standpoint;
happy to go with whatever's simplest for the DBA team. If it turns out to be
non-trivial (e.g. requires re-validating downstream reports that depend on
these being numeric), leaving it as-is is completely fine with us.

Thanks,
Manikant

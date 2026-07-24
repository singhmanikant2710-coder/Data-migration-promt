Hi Geoffrey, quick clarification on UAT #151 (CRO START lock) before I build it.
I've traced how the field works — CRO START maps to [02_CORE_02_Reviews].[Start_date], and the page already has both patterns I'd need: a "lock once set" behaviour (used for Reviewer Name, which locks once CRO Complete is populated) and role-based gating (used for MGR Approval, which only Directors and Managers can edit). So this is straightforward to implement.
One thing to confirm — the scope of the lock:
Once CRO START has been set, should it lock only for CRO and CRA users, with Directors and Managers still able to change it (the same way MGR Approval works today)? Or should it lock for all roles once set, with changes only possible via the Unlock Review flow?
I want to make sure we don't lock Directors and Managers out of correcting a date if it's entered wrongly.
Thanks, Manikant

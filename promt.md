Subject: Review Form — Save behavior (your questions on data loss)

Hi Geoff,

Great questions — since our reviews are narrative-heavy, protecting against 
data loss is worth thinking through carefully. I looked into the current 
behavior. Here's a detailed answer to each:

How saving works today: The Review Form saves manually (via the Save button). 
While you're editing, your changes are held in memory and written to the 
database only when you click Save. One positive: moving between tabs/sections 
within the Review Form preserves your changes as you go.

Q1 — Navigating away (Home, Review Home, left-nav links):
- Left-nav links that switch sections within the Review Form: your changes are 
  kept — safe.
- Home / Review Home (leaving the form entirely): unsaved changes are lost, and 
  there's no warning before you leave. This is a genuine risk.

Q2 — Closing the browser/tab: Unsaved changes are lost. There's currently no 
"you have unsaved changes" prompt when closing.

Q3 — 240-minute session expiry: If the session expires and re-login is required, 
any unsaved in-memory changes are lost. There's no warning before expiry and no 
save-on-expiry today.

So in short: within the form you're protected, but leaving the page, closing the 
browser, or a session timeout will lose anything not yet saved — currently 
without warning.

Q4 — Additional safety features we could add:
There are several safeguards we could introduce, and I think they'd be valuable 
given how much narrative work goes into each review. A few possibilities:
- An "unsaved changes" warning when leaving the page or closing the browser.
- Auto-save / draft persistence so in-progress narratives survive a reload, 
  accidental close, or session timeout.
- A session-expiry warning a few minutes before the 240-minute timeout.
- A "Saved / Unsaved changes" status indicator.

These would be new enhancements rather than fixes to existing behavior, so I'd 
like to scope them properly with the team first to make sure we do them right 
(especially auto-save, which touches how drafts are stored). Let me review the 
options internally and I'll come back to you with a recommended approach and 
timeline. In the meantime, the safest practice is to click Save before 
navigating away or if you'll be idle for a while.

Thanks!
Manikant



Subject: Geoff's request #177 — Review Form data-loss safeguards (new 
functionality, need your input before proceeding)

Hi [Manager],

Geoff raised item #177 about data loss on the Review Form (reviews are 
narrative-heavy). His first three points were questions about current behavior, 
and the fourth asks us to think through additional "safety save" features. Since 
these would be new functionality (not bug fixes), I wanted to check with you 
before committing anything to Geoff.

Current behavior (I investigated the code):
The Review Form uses manual save only — edits are held in memory 
(FormChangesContext) and persisted only on the Save button. Switching tabs 
within the form preserves changes, but navigating away (Home/Review Home), 
closing the browser, reloading, or a 240-minute session expiry all lose unsaved 
edits, with no warning. There's no auto-save or unsaved-changes guard today.

New features Geoff's request implies (all net-new):

1. Unsaved-changes warning — prompt on leaving the page / closing the browser / 
   clicking Home. Covers Q1 + Q2. Effort: Medium (frontend only). Highest 
   value-to-effort.

2. Auto-save / draft persistence — persist in-progress narratives (e.g. to 
   localStorage or a backend draft) so they survive reload/close/session expiry. 
   Covers Q3 + narrative protection. Effort: Medium-High. May need backend draft 
   support — would want to confirm architecture.

3. Session-expiry warning — warn a few minutes before the 240-minute timeout, 
   optionally auto-saving. Effort: Medium-High. Touches the auth layer.

4. Save status indicator — "Saved / Unsaved changes" indicator. Effort: Low 
   (frontend, from buffer state).

My recommendation:
Start with Feature 1 + Feature 4 (quick, high-impact data-loss protection), then 
consider Feature 2 for the narrative fields specifically. Feature 3 touches auth 
so I'd scope it carefully.

What I need from you:
1. Which features should we take on (all, or a subset like 1 + 4 first)?
2. Priority / timeline relative to Geoff's other open UAT items?
3. For Feature 2 (auto-save): are you okay with a localStorage draft approach, or 
   should drafts be backed by the database (would need Brijesh/Jothi to confirm 
   architecture)?
4. Any existing session-handling we should reuse for Feature 3?

I've told Geoff I'll scope this with the team and come back with an approach and 
timeline — so nothing is committed until we align. Let me know how you'd like to 
proceed.

Thanks,
Manikant

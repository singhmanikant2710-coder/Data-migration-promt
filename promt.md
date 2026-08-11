READ-ONLY. Diagnostics only. Do not change anything.

Investigating the Review Form's SAVE behavior to answer client questions about 
data loss (reviews are narrative-heavy). Show current behavior (no edits):

1. SAVE MECHANISM: Is there auto-save, or only manual save (a Save button)? 
   Show how the Review Form saves — look in:
   - frontend/src/app/review/[ecif]/review-info/page.tsx
   - frontend/src/components/shared/tab-flow/TabFlowProvider.tsx
   - ReviewInfoSection.tsx (the saveReview / form-changes buffer)
   Show whether edits are buffered locally (form-changes buffer) and saved 
   only on an explicit action, or saved automatically on change/blur.

2. NAVIGATE AWAY (Q1): When the user clicks Home, Review Home, or a left-nav 
   link, is there any handler that saves pending changes or warns about 
   unsaved changes before navigating? Show any beforeunload, route-change 
   guard, onBlur save, or "unsaved changes" prompt. Or do pending buffered 
   changes get lost on navigation?

3. BROWSER CLOSE (Q2): Is there a window beforeunload / onbeforeunload handler 
   that warns or saves before the browser/tab closes? Show if present.

4. SESSION EXPIRY (Q3): There's a 240-minute login period. Show how session 
   expiry is handled — is there any warning before expiry, any auto-save on 
   expiry, or do unsaved buffered fields just get lost when re-login is 
   required? Show any session/token expiry handling in the app.

5. FORM-CHANGES BUFFER: Show how the buffer works — where pending edits live 
   before save (in-memory React state? localStorage?), and whether they 
   survive a page reload or navigation. (If it's only in-memory React state, 
   navigation/close/expiry loses them.)

Do not edit anything. Show the save mechanism, navigation handling, browser-
close handling, session-expiry handling, and where pending edits are stored. 
Findings only.

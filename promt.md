READ-ONLY. Find the Add New Month success handler and whether maxMonthKey/newMonthKey update immediately after adding. Quote with paths.

ISSUE: In legacy, adding month 202608 immediately updates New Month to 202609 (same time, no refresh). In our app, after adding 202608, New Month stays 202608 even after a page refresh. So maxMonthKey isn't updating to the newly-added month.

In frontend/src/app/blackbook/edit/page.tsx:
1) Find the Add New Month button's onClick / the add-month success handler. Quote it fully. After the add API succeeds, what does it do — refetch series? refetch maxMonthKey? update selectedMonthKey? Quote all state updates post-add.
2) Quote how newMonthKey is computed (the effect: const mk = maxMonthKey; setNewMonthKey(nextMonthKey(mk)); deps [maxMonthKey]).
3) Quote how maxMonthKey is fetched — getMaxMonthKey(name) — and its effect's dependency. Is it ONLY [name] (so it never refetches after add)?
4) After adding 202608, for New Month to become 202609, maxMonthKey must update to 202608. Does any post-add code call getMaxMonthKey again or setMaxMonthKey(newlyAddedMonth)? Quote or confirm absent.
5) Also: even on REFRESH, New Month stays 202608 — so is maxMonthKey coming back stale from getMaxMonthKey (backend)? Quote getMaxMonthKey backend if the newly-added row (202608) exists in DB — does getMaxMonthKey return MAX(strMonthKey) which should now be 202608?

OUTPUT:
- A) Add New Month success handler + all post-add state updates, quoted.
- B) Does it update maxMonthKey after add? yes/no.
- C) On refresh, does getMaxMonthKey return the new max (202608)? Or stale? (If 202608 is in DB, MAX should return it.)
- D) Why New Month stays 202608: maxMonthKey not updated post-add AND/OR getMaxMonthKey returning stale.
- E) Exact fix: after successful add, setMaxMonthKey(addedMonthKey) immediately (so newMonthKey recomputes to added+1), matching legacy's instant update.
- No fix. Findings only.

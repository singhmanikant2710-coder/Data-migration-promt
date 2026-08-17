Bug #178 has two parts:

PART 1 (simple rename — proceeding): "Review Home" button -> "Review Queue" (it 
navigates to the Review Queue). Straightforward label fix.

PART 2 (Geoff's idea — NEW FUNCTIONALITY, needs your call): Geoff asks whether 
the button could instead return the user to whichever screen they came from 
(Review Queue / Review Progress / Review History), ideally with the same 
filters they had — like a browser back button. He himself framed it as "asking 
a lot, just curious about options."

This is a new feature, not a bug fix. Rough scope:
- Track the origin screen when the user opens the Review Form (query param or 
  stored state).
- On "back", navigate to that origin screen.
- Hardest part: preserve each origin screen's filters/state so it looks like 
  they left it (serialize + restore filters per screen).
Effort: Medium-High (the filter-state preservation is the bulk of it).

Options I'd propose:
- Option A (minimal): Just rename to "Review Queue" (Part 1) — always goes to 
  Review Queue. Simplest, ships now.
- Option B (medium): Dynamic label + navigation — button returns to the origin 
  screen (Review Queue/Progress/History) but WITHOUT restoring filters (screens 
  load fresh).
- Option C (full): Origin screen + filter/state restoration (the full "browser 
  back" experience Geoff described).

Per our process (new functionality needs team sign-off before building), 
holding Part 2 for your direction. Part 1 (rename) I'll proceed with unless you 
say otherwise. Which option (A/B/C) should we pursue for Part 2?

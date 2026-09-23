Got it, thanks — that timing makes sense (fix before final cutover, not before).

On re-deploying to DEV/QA: not needed on our end for now. We've already tested and verified the code fixes against the current dev data, and honestly the 4 customers' existing data (with the old convention) has been useful for that — it's what let us catch this in the first place. No need to refresh it in the interim; we're good testing against what's there now.

One important flag before this goes to QA, though: please make sure testers do NOT edit/save any months for those same 4 customers (Bankers Healthcare Group LLC, Keystone Private Income Fund, Nationwide Specialty Finance Inc, Vermeer Mountain West Inc) until the fiscal-year question is resolved. Here's why — our fix applies the standard convention on save, but only to the row being saved, not the customer's whole history (that full-history cascade isn't built yet). So if someone edits even one month for these customers right now, that one row flips to the new convention while the rest of that customer's history stays on the old one — which will look exactly like a data inconsistency bug to anyone testing it, even though it's expected and temporary. Wanted to flag it now so it doesn't get raised as a new defect by mistake. Happy to have this communicated to QA however works best on your side.

That closes out the legacy-fix side for us. Still just waiting on your read on the datFiscalYearStart question whenever you get a chance — that's the one piece left blocking our fix.


Good question — splitting this into what's solid vs. what's still open, rather than giving a blanket yes.

The fiscal year/month numbering itself — rolling from the last month of one fiscal year into the first month of the next, year incrementing correctly, elapsed-days resetting — that's core to today's fix and we've tested it across real year-end transitions for several customers, cross-checked against legacy. That part is solid.

One related thing that's still open, though: we flagged during testing that the TTM (trailing-twelve-month) calculations look like they may not roll smoothly across a fiscal-year boundary — the aggregate appears scoped to a single fiscal year rather than rolling the trailing 12 months across the boundary the way TTM is supposed to. We haven't confirmed or fixed that yet; it's a separate, not-yet-resolved item.

One clarifying question back: is there also a scheduled/batch "year-end close" process in legacy that runs when a fiscal year finishes — something beyond just the month-to-month save logic? We haven't seen evidence of one in what we've reviewed so far, but want to make sure we're not missing a requirement if that's what you meant.

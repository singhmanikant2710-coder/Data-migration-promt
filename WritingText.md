D1: A – block both (changed code clashing with another row, and a new row using a code already on the review) with a clear message naming the code; nothing is written.
D2: A – block swaps/chains in one save with a clear message ("save one change at a time").
D3: B – block with a message: "Select a Finding Code for the changed CRM Component." Nothing is written; edits stay on screen.
D4: A – deleting a never-saved row is local only (no server call); re-key saved rows with an in-place UPDATE inside the save transaction.
All blocks happen before any write (after the #212/#249 checks, before the #232 version claim, same as #247), so a rejected save changes nothing and keeps the user's edits.
Proceed with Phase 3 and 4.

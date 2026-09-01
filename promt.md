Before applying, confirm ONE thing: in UpsertRowWithConnectionAsync, is the `exists` boolean already computed/assigned BEFORE the point where we're inserting the new `if (!exists) { ... prevMonth override ... }` block (which is after ApplyIncomingCovenantsToSlotsAsync and before the `if (exists) {...} else {...}` split)?

Quote the line where `exists` is assigned (e.g. `bool exists = ...` or `var exists = await ...`). Confirm it appears BEFORE our inserted block. If `exists` is computed AFTER that point, the override won't compile / will use a wrong value — in that case, tell me where `exists` is set so we can place the override correctly (after exists is known, before fy/fm are used in INSERT).

Just confirm: is `exists` set before our inserted `if (!exists)` block? Quote the assignment line and its position relative to our insert point.

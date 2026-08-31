READ-ONLY. One focused question. Find why threshold/description edits don't reach covEdits (so payload is empty and falls back to {order}).

CONFIRMED: by-name payload is only { order: 3 }. The construction reads from covEdits per row. For existing MDR covenants (which HAVE names like "Max Debt/EBITDA"), the user edits threshold/description, but the payload ends up empty → falls to creation-only { order }. So covEdits for these rows must lack threshold/description at save time.

1) In onCovenantRowEdit (page.tsx): when threshold is edited on an EXISTING row (has originalName from row.name), does the patch { threshold: v } get merged into covEdits[key]? Quote the key construction and merge. Is the key stable between the threshold edit and save (id vs composite key)?
2) Is there any code that RESETS or CLEARS covEdits — e.g. after an order change, a re-render, a rekey, or the order-save flow? Quote any setCovEdits({}) or covEdits mutation outside onCovenantRowEdit. Could saving/order-rekey wipe field edits before onSaveCovenantFields runs?
3) In onSaveCovenantFields: does it iterate covEdits entries? For an existing row with a threshold edit, would patch.threshold be present? Quote the entries source (Object.entries(covEdits)?) and whether it's read fresh at save time.
4) The "order rekey logic omitted" in onCovenantRowEdit — does it re-key covEdits entries (change the key) when order changes, potentially orphaning or dropping the threshold patch? Quote that rekey logic.
5) Sequence: if the user changes ORDER and also edits threshold, does the order handling overwrite/replace the covEdits entry (losing threshold)? Quote.

OUTPUT:
- A) covEdits key + merge for a threshold edit on existing row, quoted.
- B) Any covEdits reset/rekey/clear that could drop field edits, quoted.
- C) At save, is threshold present in the covEdits entry for existing rows? If not, why (dropped/rekeyed/cleared).
- D) Exact fix location.
- No fix. Findings only.

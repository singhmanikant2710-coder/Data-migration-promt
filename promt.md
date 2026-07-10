Bug: The cascade is not triggering. When I change the Segment dropdown (in edit mode), NO new "units?segment=..." call is made. The units useEffect (dependency [selectedSegment]) is not firing on segment change.

READ-ONLY diagnostic first (no edits):
1. Show me how SelectField reports a value change. When the user picks a new Segment, what exactly gets updated? Does it call changes.setField("customerInfo","segment", newValue), or an onValueChange/onChange callback?
2. In CustomerInfoSection.tsx, how is selectedSegment derived? Is it `r?.segment`? If so, does `r` (the review object) update immediately when the user changes the dropdown in edit mode, or does it only reflect saved/server data?
3. The problem is likely that selectedSegment reads from `r?.segment` which does NOT update on in-form edits, so the [selectedSegment] effect never sees the change. 
4. Propose a fix: selectedSegment (and selectedUnit) must reflect the CURRENT in-edit form value (from changes/edit state), not the saved `r` object — so the cascade effects fire when the user changes the dropdown. Show how other SelectFields in this app read their current edited value, and follow that pattern.

Report findings and proposed fix. Do not edit yet.

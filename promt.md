
The Segment, Unit, and Market fields in the Relationship Overview are still rendering as plain read-only text (Field), NOT as dropdowns (SelectField). The cascade wiring did not get applied. Compare: Customer Size, FHN Portfolio Segment, FHN NAICS Industry all show dropdown arrows, but Segment/Unit/Market do not.

READ-ONLY diagnostic first (no edits):
1. Show me the CURRENT exact JSX for Segment, Unit, and Market in CustomerInfoSection.tsx. Are they <Field> or <SelectField>?
2. If they are still <Field>, the earlier cascade edit was not applied to these three fields. 
3. Also confirm: is the getLookupOptions helper able to pass query params (segment, unit)? And what are the exact backend routes for relationship-segments/units/markets?

Report findings. Then propose the single-file edit to CustomerInfoSection.tsx to convert these three to cascading SelectFields (with the [], [segment], [segment,unit] effects we planned), plus any helper/route fix needed. Wait for my confirmation before editing.

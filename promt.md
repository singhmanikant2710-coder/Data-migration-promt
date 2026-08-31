READ-ONLY. Find how the covenant save payload (effectivePayload / CovenantUpdateReq) is built, and why it only contains "order" — not threshold, reported, description. Quote with paths.

CONFIRMED VIA NETWORK: The PUT /covenants/by-name call succeeds (204) but its payload is ONLY { order: 3 } — threshold, reported, description are NOT in the payload. So the field values are never sent, and DB keeps them NULL/0. The /covenants/order call also only sends ordering.

In frontend/src/app/customer/edit/page.tsx:
1) Find where effectivePayload (the CovenantUpdateReq passed to updateCovenantByComposite / updateCovenantById) is BUILT. Quote the full construction. What fields does it include? Why only "order"?
2) Where are the covenant row's threshold, reported, description values held in state (as the user types)? Quote the state and the input onChange handlers for threshold/reported/description.
3) Trace: when the user edits threshold and clicks Save, are threshold/reported/description read from state and added to effectivePayload? Or is effectivePayload built only from the order/dirty-order tracking, omitting field values?
4) Is there separate dirty-tracking for order vs field values? Maybe only order changes are collected into the payload, and field-value changes are tracked elsewhere (or not at all). Quote the dirty/diff logic that decides what goes into effectivePayload.
5) Quote the CovenantUpdateReq type — what fields CAN it carry (order, threshold, reported, description, etc.)? So we know the payload could include them if we add them.

OUTPUT:
- A) effectivePayload construction, quoted. Which fields it includes and why only order.
- B) Where threshold/reported/description are held in state + their onChange, quoted.
- C) Why they're not in the payload: (i) not read from state into payload, (ii) only order is dirty-tracked, (iii) built from wrong object.
- D) CovenantUpdateReq type fields, quoted (to confirm it can carry threshold/reported/description).
- E) Exact fix location — where to add threshold/reported/description into effectivePayload.
- No fix. Findings only.

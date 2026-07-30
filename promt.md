Re: #170 — confirmations
Thanks Geoff, Option A it is. Answers to your questions:
1. Selections maintenance screen ordering — no impact. That screen's main grid is driven by a separate query and is sorted client-side by Tab → Section → Selection ID. My change only touches the per-(tab, section) values query, which feeds the value dropdowns during edit/add. So the Selections screen will keep its current Tab → Section → Selection ID order exactly as-is.
2. FHN Portfolio Segment — noted, we're good.
3. PD Grade / LGD Grade — confirmed, no impact to grade order. I verified against the Selections table: for both Scorecard → PD Grade and Scorecard → LGD Grade, the Selection_id sequence already matches the numerical grade order (1, 2, 3 …). Since the new sort is by Selection_id ascending, these dropdowns will render in the same numerical order they do today. You're right — no real change there.
So the only visible reordering will be the Report Name Selection dropdown (and it now matches the intended Selection_id sequence). I'll implement and let you know once it's ready for QA.

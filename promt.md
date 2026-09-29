REGRESSION from the class-B change: Cash & Charge-offs "Fixed Charges
TTM" shows $0 for ATHENS 202604, before and after save. DB has
curFixedChargesTTM = 1568.299 (202603 = 1613.731), and FCC TTM 5.10
already uses it. So the tile reads the wrong key/source.

1. Quote file:line of the Fixed Charges TTM tile and exactly which
   key/object it reads, and why it resolves to 0 now (e.g. value was
   produced by the frontend calc loop that now skips class B).
2. Fix generically: every class-B field shows the saved value from the
   row (stored column); check all CLASS_B_CALCS fields for the same
   problem (compare tile value vs DB for ATHENS 202604).
3. Class B with no saved value renders "—", not 0 (e.g. A/R Turn Days,
   Inventory Turn on a new unsaved month).
Build, tests, do not commit.
Report ADDED/REMOVED, BEHAVIOUR CHANGE incl. NULL, NOT TOUCHED, and a
table: field | DB value (202604) | UI before | UI after.

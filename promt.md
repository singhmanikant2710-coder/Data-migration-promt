Apply this edit now. Do not read other files beyond what you need. Do not plan. Just apply.

FILE: frontend/src/app/review/[ecif]/review-info/components/sections/CrmFindingsAndRatingsSection.tsx

Problem: when a Finding Code is selected and saved, the CLOSED dropdown field shows the full "CS-104 - Covenant compliance monitoring lacks timeliness..." label. The client only wants the code.

Fix: on the SearchableSelect used for Finding Code, pass a renderSelected prop that returns ONLY the raw code (e.g. "CS-104") — never the description.

The description must still appear in the OPEN dropdown list (do not change that).
The option VALUE stays the raw finding code — the save path must not change.

Apply this single edit and show me the diff. STOP.

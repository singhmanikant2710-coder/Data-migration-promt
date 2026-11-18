UAT #255 – Review Form label change: Regulatory Tracking

REQUIREMENT
In the Review Form (Regulatory Tracking tab), the label of the field bound to dbo.[02_CORE_02_Reviews].Regulatory_flag_tracking_accuracy must change from "IS REGULATORY TRACKING ACCURATE" to "ARE FLAGS APPROPRIATE AND IDENTIFIED".

PHASE 1 – READ-ONLY DISCOVERY (no edits)
- file:line of the Review Form label for Regulatory_flag_tracking_accuracy.
- Search the whole repo (frontend, backend, PDFs in components/pdf, report repositories, exports, help tips/seed SQL, tests) for "Is Regulatory Tracking Accurate" (any case) and for Regulatory_flag_tracking_accuracy. List every hit with file:line and what it is (label, PDF heading, report column, tooltip/help tip, CSV header).

PHASE 2/3 – CHANGE
- Change ONLY the Review Form label text (keep the same casing style as the other labels on that tab). No change to the field, value, save logic or column.
- Do NOT change PDFs, reports, exports or help tips – list them for me as a decision (with a recommendation) instead.

PHASE 4 – REPORT (in chat, not in a file)
- CHANGED (file:line), before/after label text, list of other hits not changed.
- npx tsc --noEmit, npm run build.
- git status (restore next-env.d.ts / tsconfig.tsbuildinfo if rewritten; leave pre-existing SQL scripts and devin-output/ alone).
- Do not commit or push.

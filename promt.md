Skeleton approved. Two confirmations:

1. Customer-info row stays exactly the 7 fields as scoped (Customer Name, Customer #, Review ID, Unit, Market, RM Name, PM Name) — no Segment/Sample/Exposure, no additional join. The prototype only shows these 7.

2. Noted on Risk_recognition_comments being shared with InitialMemo/FinalMemo's fallback logic — since this report only reads it, no action needed, no regression risk.

Proceed: implement the per-review block body in the skeleton, plus the full backend pipeline (4 new files + controller method + 2 DI lines) exactly as designed. Show the full diffs (frontend block-rendering + all backend files). Rebuild, run typecheck. Do NOT commit.

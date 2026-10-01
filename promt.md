Follow-up: doSendServer (review-info/page.tsx:260-269) sends one email per
To recipient, each with the full CC list. With To now pre-filled with RM +
PM, every CC recipient gets duplicate copies.

1. Investigate the backend send endpoint used by sendEmail: does it accept
   multiple To addresses in one message? Report file:line.
2. If yes: send ONE email with all To addresses and all CC addresses.
   If no: extend the endpoint to accept a list of To addresses (generic,
   backward compatible: a single To must keep working for any other
   caller), then send one email.
3. Keep everything else unchanged: attachment, subject, body, validation,
   success/error toasts, and the post-send reset.

Report ADDED / REMOVED (file:line), behaviour for: 1 To + 3 CC, 2 To + 3 CC,
2 To + 0 CC (each recipient must receive exactly one email), the NULL case
(To empty: Send stays blocked with a clear message, as today), any other
callers of the endpoint and confirmation they're unaffected, and the build
results. Do not commit.

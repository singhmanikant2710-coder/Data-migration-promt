Line 320 uses parts.join(" ") — that removes the " - " between code and
title. It must stay parts.join(" - "); only the separator before the
count changes. Expected: "RR-101 - Directed Downgrade (1)". Fix that
line only. Report ADDED/REMOVED (file:line), and the same NULL-case
checks. Build result. Do not commit.

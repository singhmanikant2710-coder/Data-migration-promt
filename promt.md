Single-file edit: frontend/src/app/review/[ecif]/review-info/components/sections/ReviewInfoSection.tsx

Change the Unlock modal's default selected option from "APPEAL" to "GENERAL" 
(client wants "Unlock for General Revisions" as the default — most common).

Change:
    const [unlockReason, setUnlockReason] = useState("APPEAL");
to:
    const [unlockReason, setUnlockReason] = useState("GENERAL");

CONSTRAINTS:
- ONLY change the default useState value from "APPEAL" to "GENERAL".
- Do NOT change anything else.
- Only edit this one file. Show the changed line.

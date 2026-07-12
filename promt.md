Your code review says STEP 2 is correct, but the RUNTIME behaviour is still broken — the unsaved row still disappears on tab switch. So the condition is failing at runtime. Stop re-reading the code and answer this specific question.

In `rowsToRender` the condition is:
    if (isEditing && Array.isArray(pending)) return pending;
    return s?.findings ?? [];

QUESTION: after the user switches to CRM Ratings and comes BACK to CRM Findings, is `isEditing` still TRUE, or has it reset to FALSE?

Trace it: show me exactly where `isEditing` comes from, where it is set, and whether it survives a section switch (the section remounts, and the Edit/Save/Cancel toolbar lives in the parent page). If `isEditing` resets to false on remount, then the pending snapshot is ignored and the table falls back to saved data — which exactly matches the bug.

If that is the cause, the fix is: `rowsToRender` should use the pending snapshot whenever one EXISTS, regardless of `isEditing` — because a pending snapshot by definition means there are unsaved changes that must be shown. i.e.:

    const rowsToRender = useMemo(() => {
      const pending = changes?.changes?.crmFindingsAndRatings?.findings;
      if (Array.isArray(pending)) return pending;
      return s?.findings ?? [];
    }, [changes?.changes?.crmFindingsAndRatings?.findings, s?.findings]);

Confirm whether `isEditing` is the blocker, and if so apply the fix above (single file, CrmFindingsAndRatingsSection.tsx). If `isEditing` is NOT the blocker, tell me what actually is — with evidence, not a code re-read.

Show me the diff. STOP for approval.

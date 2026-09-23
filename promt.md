That's an important correction, thank you — changes our understanding materially.

This means the current TTM logic (which looks like it's scoped by fiscal year) is wrong on the axis, not just at the boundary — it needs to be a pure calendar rolling window with no fiscal-year dependency at all.

The look-back/look-ahead point is the bigger one for us: if editing one month's value can affect TTM for up to 11 months forward (and possibly further back too, if there's an onboarding-period fallback before 12 months of history exist — we'll check a recently-onboarded customer to confirm), that's a much wider recompute scope than anything in today's fix or the fiscal-start cascade we designed. This isn't a small patch.

We're going to dig into the current code and check early-customer data as you suggested. Given the scope, I don't think this should get folded into tomorrow's delivery without proper verification — want to flag that now rather than rush it. Can we track this as a separate item, with tomorrow staying scoped to what's already fixed and verified (the 5 bugs) plus the fiscal-start cascade once the datFiscalYearStart question is settled?

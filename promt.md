SINGLE-FILE, BOUNDED EDIT. Only frontend/src/app/blackbook/edit/page.tsx, only the principalGrossByLabel seeding effect (~L1898-1934). Show unified diff BEFORE applying. Do not run build until I confirm.

CONTEXT: The three Principal/Gross selector strings ALREADY arrive on the client — MapMetricPoint whitelists strprincipalorgross* and MetricPoint.Values is Dictionary<string,object?>. So point.values["strPrincipalOrGrossCalculationSelectionper60DPD"] is already "Principal N/R" or "Gross N/R". NO backend change, NO guard needed.

THE ONLY BUG: principalGrossByLabel seeds from a hardcoded default ("Principal N/R") instead of the customer's stored selection. This makes the dropdown wrong for the ~7-11% Gross customers.

FIX: Seed principalGrossByLabel from the latest row's stored selector strings, falling back to the hardcoded default only when the stored value is blank/absent.

Current (~L1905-1934):
    const opts = await getPrincipalOrGrossOptions();
    const options = (Array.isArray(opts) && opts.length > 0) ? opts : ["Principal N/R"];
    const def = options.find(o => /principal/i.test(o)) || options[0];
    if (cancelled) return;
    setPrincipalGrossOptions(options);
    setPrincipalGrossByLabel(prev => {
      const next = { ...prev };
      ["Cash Collections $", "Cash Collections %", "Net C/O $", "Net C/O %", "60+ DPD $", "60+ DPD %"].forEach(lbl => {
        if (!next[lbl]) next[lbl] = def;
      });
      return next;
    });

Change to seed from the stored selectors (read from latestPoint?.values via `as any`, since the TS type says number|null but strings arrive at runtime). Map each stored selector to its two labels ($ and %). Fall back to `def` only when blank:

    const opts = await getPrincipalOrGrossOptions();
    const options = (Array.isArray(opts) && opts.length > 0) ? opts : ["Principal N/R"];
    const def = options.find(o => /principal/i.test(o)) || options[0];
    if (cancelled) return;

    // Read the customer's stored basis from the latest row (selectors already arrive on values).
    const lv = (latestPoint?.values || {}) as any;
    const storedCash = String(lv["strPrincipalOrGrossCalculationSelectionCashCollection"] ?? "").trim();
    const storedDpd  = String(lv["strPrincipalOrGrossCalculationSelectionper60DPD"] ?? "").trim();
    const storedNco  = String(lv["strPrincipalOrGrossCalculationSelectionNetChargeOff"] ?? "").trim();
    // Normalize a stored value to a valid option, else fall back to the default.
    const norm = (stored: string) => {
      const s = stored.toLowerCase();
      if (!s) return def;
      const match = options.find(o => o.toLowerCase() === s)
        || (/(gross)/i.test(stored) ? options.find(o => /gross/i.test(o)) : undefined)
        || (/(principal)/i.test(stored) ? options.find(o => /principal/i.test(o)) : undefined);
      return match || def;
    };
    const cashSel = norm(storedCash);
    const dpdSel = norm(storedDpd);
    const ncoSel = norm(storedNco);

    setPrincipalGrossOptions(options);
    setPrincipalGrossByLabel(prev => {
      const next = { ...prev };
      const seed: Record<string, string> = {
        "Cash Collections $": cashSel, "Cash Collections %": cashSel,
        "Net C/O $": ncoSel, "Net C/O %": ncoSel,
        "60+ DPD $": dpdSel, "60+ DPD %": dpdSel,
      };
      Object.entries(seed).forEach(([lbl, val]) => {
        if (!next[lbl]) next[lbl] = val;   // preserve any manual user change (existing guard)
      });
      return next;
    });

ALSO: add latestPoint (or selectedMonthKey) to this effect's dependency array so it re-seeds when the row arrives (currently it may run before row data). Keep the `if (!next[lbl])` guard so a user's manual dropdown change is never overwritten.

STRICT / no-regression:
- Only this effect changes. No other code, no other file, no backend.
- The `if (!next[lbl])` guard is preserved — manual dropdown changes are never overwritten.
- Fallback to `def` (Principal) only when the stored selector is blank — so today's behavior is unchanged for blank-selector customers (the 89-93%), and Gross customers now seed correctly.
- Do NOT add the Option (b) clobber guard (unnecessary — selectors reach the client, calcs compute correct basis).
- Do NOT change MetricPoint type or backend.

VERIFY BEFORE SHOWING DIFF:
a) Seeds from stored selectors (Cash/DPD/NetC/O), falls back to def only when blank.
b) Both $ and % labels seeded per metric.
c) if (!next[lbl]) guard preserved (manual changes safe).
d) latestPoint/selectedMonthKey added to dependency array.
e) No backend/type change; only this effect.

Show the unified diff. Apply nothing until I confirm.

Single-file edit to useCrmFindings.ts only. Show diff before applying.

The previous guard change broke labels for all components. Revert to a clean, correct version of the label-building effect. Replace the ENTIRE effect with this:

useEffect(() => {
  let isCancelled = false;
  const comps = Object.keys(codeMap) as CrmComponentId[];
  for (const comp of comps) {
    const codes = codeMap[comp] ?? [];
    if (!comp || codes.length === 0) continue;
    if (inFlight.current.has(comp)) continue;
    const existing = labelMap[comp];
    const alreadyDone = existing && Object.keys(existing).length > 0
      && Object.values(existing).some(v => typeof v === "string" && v.includes(" - "));
    if (alreadyDone) continue;

    inFlight.current.add(comp);
    listCasFindingsLibrary(comp)
      .then((items) => {
        const map: Record<string, string> = {};
        if (Array.isArray(items)) {
          for (const it of items as any[]) {
            const code = String(it?.findingCode ?? it?.FindingCode ?? "").trim();
            if (!code) continue;
            const desc = String(it?.description ?? it?.Description ?? "").trim();
            map[code] = desc ? `${code} - ${desc}` : code;
          }
        }
        if (!isCancelled) setLabelMap((prev) => ({ ...prev, [comp]: map }));
      })
      .catch(() => {
        const map: Record<string, string> = {};
        for (const c of codes) map[c] = c;
        if (!isCancelled) setLabelMap((prev) => ({ ...prev, [comp]: map }));
      })
      .finally(() => { inFlight.current.delete(comp); });
  }
  return () => { isCancelled = true; };
}, [codeMap, labelMap]);

Do NOT touch save logic or option value (value stays = code). No other file. Show diff. STOP if another file needs changing.

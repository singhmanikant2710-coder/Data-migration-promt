Two fixes in computeConsumerFinancePercentOverride in frontend/src/app/blackbook/edit/page.tsx:

FIX 1 (CRITICAL - duplicate variable, build error): There are TWO identical "const denom = ..." declarations. Remove the SECOND one. Keep only ONE:
    const denom =
        pick(values as any, useGross ? grossAliases : prinAliases) ??
        pickMonthly(values as any, useGross ? grossAliases : prinAliases);
Delete the duplicate second "const denom = ... pickMonthly(...);" block entirely.

FIX 2 (base label extraction): Change:
    const base = lbl.replace(/^\s*%/, "");
to:
    const base = lbl.replace(/\s*%$/, "");
(The % is at the END of the label like "Cash Collections %", so strip trailing %, not leading.)

After fixing, show me the complete function again from signature to closing brace. It must have exactly ONE "const denom" and use /\s*%$/ for base. Confirm no duplicate declarations and valid syntax. Do not run build yet.

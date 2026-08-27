Apply the diff exactly as shown. Only change the final guard in computeConsumerFinancePercentOverride in frontend/src/app/blackbook/edit/page.tsx:
Replace:
    if (nn === null || dd === null || dd === 0) return null;
    return (nn / dd) * 100;
With:
    if (nn === null) return null;
    if (dd === null || dd === 0) return 0;
    return (nn / dd) * 100;
Nothing else changes. Apply now and confirm.

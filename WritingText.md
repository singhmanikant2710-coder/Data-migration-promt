Hi Samuel,

I've proposed "Mitigate by Design" for three Veracode findings in CASRR (webapp-casrr, sandbox casrr/api-qa): #59, #60 and #61 in ReviewPDFModal.tsx (CWE-80). They are false positives – the value is a browser-generated blob: URL for a client-generated PDF, with no user or database data reaching the sink. The full justification is in each mitigation comment.

Could you please review and approve, or let me know who the right approver is?

Thanks,
Manikant

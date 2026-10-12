This is a DOM node append, not an HTML sink. The anchor is created with document.createElement("a") and only href (the browser-generated same-origin blob: URL for the client-generated PDF) and download (a filename restricted to letters, digits, underscore and hyphen) are set. No string-built HTML (innerHTML, outerHTML, insertAdjacentHTML, document.write) is used. The node is appended, clicked and removed synchronously to start the PDF download.


None identified. No untrusted content can be injected into the document through this call.


Code review of ReviewPDFModal.tsx confirms createElement + property assignment only and immediate removal after click. Manual test in QA: PDF download works and no element remains in the DOM afterwards.

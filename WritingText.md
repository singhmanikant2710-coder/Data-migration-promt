The iframe src is the same-origin blob: URL created by URL.createObjectURL for a PDF generated in the client by @react-pdf/renderer (content type application/pdf). The browser displays it with its built-in PDF viewer; it is never treated as an HTML document. The value is not user-controllable (application origin + browser-generated UUID), srcdoc is not used, and src is set through a React JSX attribute, not string-built HTML.


None identified. The PDF is generated from data the authenticated user is already authorised to view, and no script from review data can execute in the PDF viewer.


Code review of ReviewPDFModal.tsx confirms the iframe src comes only from URL.createObjectURL. Manual test in QA: the PDF preview opens in the browser PDF viewer with a blob: URL of the application origin.

Read-only. No edits. Just report.

File: frontend/src/components/pdf/HtmlRichText.tsx

The component was wired into ReviewPDF.tsx for the Customer Background section, but tables in the comments HTML are still rendering as flattened plain text in the generated PDF (e.g. "A1 B1 C1 Testing 1 Testing B1 TestingB2 12345"). This means the table branch is not executing and the plain-text fallback is being used.

Report:
1) Paste the full current source of HtmlRichText.tsx.
2) Identify exactly where the DOMParser guard is and what it falls back to. Under what conditions does it hit the fallback?
3) Is the parsing done inside the component render body, or lazily/asynchronously? Note that @react-pdf/renderer renders the component tree via pdf().toBlob() — confirm whether `window`/`DOMParser` are actually available in that execution path, or whether the guard silently fails there.
4) Is there a try/catch that swallows a real error? If so, what would it catch here?

Do not change anything. Output the source and your analysis only.

Subject: CASRR – Request to approve npm package "dompurify" for Veracode XSS remediation

Hi Team,

We are remediating the open Veracode findings for CASRR (application: webapp-casrr, sandbox: casrr/api-qa, latest scan casrr/api-20261005.3). The scan reports 14 Medium findings of type CWE-80 (Basic XSS) in the Next.js frontend, where rich-text and help-tip HTML from the database is displayed on screen without sanitisation.

What we want to apply
- Add the npm package dompurify to the CASRR frontend and sanitise this HTML before it is displayed.
- No backend, database or infrastructure change.

Why dompurify
- Industry-standard HTML sanitiser, recognised by Veracode as a valid cleanser, so the findings can be closed as Fixed.
- Removes only dangerous content (scripts, event handlers, javascript: links) and keeps normal formatting (bold, tables, images), so existing screens are unaffected.
- A custom-written sanitiser would likely not be recognised by Veracode and the findings would remain open.

Package details
- Name / version: dompurify 3.4.16 (exact version, pinned)
- Licence: MPL-2.0 OR Apache-2.0
- Dependencies: none (zero transitive packages)
- Size: approx. 22 kB minified / 8 kB gzipped in the browser bundle
- Usage: browser only (no server-side execution)
- Source: public npm registry (npmjs.com)

What we need from you
1. Confirmation that dompurify 3.4.16 is approved for use, or the process/ticket we should raise for open-source package approval.
2. Confirmation that the CI/CD pipeline / npm feed (e.g. Azure Artifacts upstream) can resolve this package, so the build does not fail after merge.
3. Any additional security requirement for new packages (e.g. SCA scan, licence review) that we should complete before merging.

Once confirmed, we will merge the change to QA and run a new Veracode sandbox scan to verify the findings move to Fixed.

Thanks,
Manikant

Small follow-up: add a client-side "To is required" check in the Share via
Email modal. When splitEmails(to) is empty, disable Send and show an inline
message under the To field (same style as the Document Type message), with
no server call. Don't change anything else.
Report ADDED/REMOVED (file:line), behaviour with empty To / valid To / invalid
To, and the build result. Do not commit.

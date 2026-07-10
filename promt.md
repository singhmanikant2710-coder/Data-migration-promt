Approved — this is the correct fix. Replace the raw fetch() in useCrmFindings.ts with the existing authenticated helper listLibrary from "@/services/api/casFindings", which attaches the auth token via @/lib/api.

Keep unchanged:
- The per-component cache (labelMap) and in-flight guard.
- The label logic: `${code} - ${description}` (fallback to `${code}` if description empty).
- Option value stays the raw Finding_code (save path untouched).

Single-file edit to useCrmFindings.ts. Apply now.

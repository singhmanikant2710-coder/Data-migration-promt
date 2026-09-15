Phase 1 — Bug 228. Build a REAL, working RequireAdmin authorization policy. READ-ONLY investigation first, then implement. Show diffs, do NOT commit.

READ-ONLY first — confirm before changing:
1. Paste the exact SampleController.cs inline admin-check (lines ~279-315) — the pattern that resolves identity → looks up user → checks AdminRights == true → returns 403. This is the logic to generalize.
2. Paste DatabaseRoleClaimsTransformation.cs in full — where does it currently read the Role string claim, and where would AdminRights need to be added as a claim?
3. Paste the existing (currently unused) Commercial.Admin policy definition in StartupExtensions.cs.
4. Confirm: is there an existing IUserRepository.ResolveByIdentityAsync (or similar) that the claims transformation or a new policy could call to look up AdminRights during token/claims processing?

THEN implement:
5. In DatabaseRoleClaimsTransformation.cs, add AdminRights as a proper boolean claim (e.g. ClaimTypes.Role with value "Admin", or a custom claim type "IsAdmin") when the user's AdminRights == true, alongside the existing Role string claim. Reuse the existing user-lookup that's already happening there — do not add a second DB call if one already exists in this transformation.
6. Add a new working RequireAdmin policy in StartupExtensions.cs that checks for that claim (mirror how RequireActiveUser is structured). Handle the authEnabled == false local-dev degradation case the same way the existing policies do (so local dev doesn't silently break, but also doesn't silently bypass the admin check in a way that masks bugs — confirm the existing degradation pattern and match it consistently).
7. Do NOT apply this policy to any controller yet — Phase 1 is just building the policy correctly and proving it works. Do NOT touch SampleController's existing inline check (leave it as-is for now; it can be migrated to use the new policy in a later, separate pass to avoid scope creep here).

Show diffs. Rebuild. Do NOT commit. This is foundational — I want to review carefully before we build Phase 2 (enforcement) on top of it.

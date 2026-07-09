Plan approved. Proceed step-by-step in the order listed. Start with step 1 (ICustomerInfoLookupRepository.cs) and pause after each file for my confirmation.

One note: the new endpoint accepts multiple roles via repeated query params (roles=PML&roles=RPML). Please make sure:
- The SQL uses a properly parameterized IN clause (one @param per role), not string concatenation.
- The controller safely handles an empty roles list (return empty array, no error).

Keep the existing fallback to userOptionsAll/userOptionsCRO if the live fetch fails, as you described. Bindings (section/name/value) stay unchanged.

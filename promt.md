Fix UAT Issue #177 – Review Form Save / Unsaved Data Loss

Please investigate and implement a robust solution for the Review Form saving/data-loss issue described below.

UAT Issue #177:
1. Can the Review Form save if the user navigates away from the review by clicking Home, Review Home, or any left-hand navigation panel links?
2. Can the Review Form save if the user closes the browser?
3. What happens to unsaved fields if the application reaches its 240-minute login/session expiry and a new login is required?
4. Are there additional safety-save features we can add to the Review Form to avoid data loss, especially because reviews contain large amounts of narrative comments and manually entered tables?

Business/User requirement:
The Review Form must minimize the risk of losing user-entered data. Users may enter substantial narrative comments or table data and may navigate away, close the browser, or encounter the 240-minute authentication/session expiry without explicitly clicking Save.

Please inspect the existing Review Form implementation and determine:
- How Save currently works.
- Whether navigation away triggers a save or warning.
- Whether browser/tab close can preserve unsaved data.
- How the 240-minute authentication/session expiry currently affects unsaved form state.
- Whether the form uses React state, localStorage/sessionStorage, autosave, draft APIs, or any existing persistence mechanism.
- Identify the safest existing architecture/pattern in this codebase and reuse it rather than introducing a completely separate mechanism.

Implementation expectations:
1. Add/strengthen autosave/draft persistence for Review Form changes where appropriate.
2. Prevent accidental data loss when navigating through Home, Review Home, or left-side navigation links.
3. Before navigation, detect dirty/unsaved changes and either save the draft automatically or provide an appropriate confirmation/warning.
4. Handle browser/tab close as safely as technically possible. Do not rely only on an unload event because browsers may terminate asynchronous requests.
5. Handle authentication/session expiry gracefully. If the session expires while the user has unsaved changes, preserve the draft locally or through the existing backend draft mechanism so the user can recover the work after re-authentication.
6. Make sure already-saved data is not duplicated or overwritten incorrectly.
7. Avoid changing unrelated Review Form functionality.
8. Follow existing project conventions for API calls, state management, error handling, notifications, and persistence.
9. Do not make database/schema changes unless the existing architecture genuinely requires them.

Important:
- First inspect the codebase and identify the actual Review Form save flow and all navigation paths.
- Do not guess the implementation.
- Reuse existing save/draft infrastructure if available.
- Keep the change scoped specifically to UAT Issue #177.
- Consider race conditions/debouncing so autosave does not generate excessive API requests.
- Make sure the user receives clear feedback when a draft is being saved, successfully saved, or cannot be saved.

Acceptance criteria:
- Entering data in the Review Form and navigating to Home/Review Home/left navigation must not silently lose the changes.
- Closing/reloading the browser should preserve recoverable unsaved work as far as browser limitations allow.
- Session expiry at 240 minutes must not silently discard unsaved Review Form data.
- Reopening/re-entering the review should allow the user to recover the latest draft where applicable.
- Existing explicit Save functionality must continue to work.
- No duplicate records or duplicate table rows should be created because of autosave.
- Existing Review Form behavior should remain unchanged when there are no unsaved changes.
- Add/update automated tests for the relevant save, navigation, draft recovery, and session-expiry scenarios.
- Build and run the relevant tests before considering the issue complete.

Please provide:
1. Root cause of the current data-loss risk.
2. Files/components/services changed.
3. Exact implementation approach.
4. Test results.
5. Any browser/authentication limitations that cannot be fully guaranteed.

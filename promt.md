Two follow-ups, same rules:
1. POST /api/v1/main/month must have exactly the same [Authorize]
   policy as PUT /api/v1/main/row. Quote both attributes.
2. The self-test page /blackbook/expr/self-tests must not ship as a
   production route. Gate it to Local/Development only (or move the
   cases to a non-routed test file). Quote the change.
Build, do not commit. Report ADDED/REMOVED, NOT TOUCHED.

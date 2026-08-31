READ-ONLY. Find why covenant FIELD values (threshold, reported, description) save as NULL/0 while only order saves. Quote with paths.

DB CONFIRMED: In tblMainCovenants for MDR CONSTRUCTION INC (202601): intCovenantOrder is saved correctly (4,2,5,6,1,3,0), but strCovenantThreshold=0, strCovenantReported=NULL, strCovenantDescription=NULL — all field values are NULL/0 even though the user entered them. So the ORDER save works (/covenants/order) but the FIELD-VALUE save (/covenants/by-name via updateCovenantByComposite) is not persisting threshold/reported/description.

In frontend/src/app/customer/edit/page.tsx (and the covenants component/lib):
1) Quote the Save button onClick handler FULLY. List every API call it makes. Does it call updateCovenantByComposite (or updateCovenantById) for EACH row's field values, OR only the /covenants/order bulk save?
2) If it calls updateCovenantByComposite, quote the CovenantUpdateReq payload it builds — does it include threshold, reported, description with the entered values? Quote the exact object.
3) When does the field-value save happen — on Save click, or on input blur/change? If on blur, is it actually firing? Is there a guard (if !cust || !mk || !cov return) that might skip it (e.g. covenantName empty)?
4) Trace one covenant (e.g. "Min Fixed Charge Coverage") with threshold=1.00: does a PUT /covenants/by-name fire with threshold=1.00 in the body? Or does that call never fire / fire with empty values?
5) Quote the CovenantUpdateReq type — field names (threshold? Threshold? strCovenantThreshold?). Do the payload keys match what the backend by-name handler reads?
6) Does the backend by-name handler actually UPDATE threshold/reported/description columns? (If we have the handler.) Or does it only update some fields?

OUTPUT:
- A) Save handler + all calls it makes, quoted. Does it save field values or only order?
- B) The CovenantUpdateReq payload for field values, quoted — are threshold/reported/description included with real values?
- C) Why fields are NULL/0: (i) by-name call never fires, (ii) payload missing/empty fields, (iii) key-name mismatch, (iv) backend doesn't update those columns, (v) guard skips it.
- D) Exact fix location.
- No fix. Findings only.


In frontend/src/app/review/[ecif]/review-info/page.tsx, apply a layout-only fix so the sticky TopChromeBar header no longer overlaps tab content on scroll. This affects all tabs via the shared wrapper.

Changes (page.tsx ONLY, layout/spacing only — no business logic, no API, no backend, no TopChromeBar edits, no packages):
1. Add: const toolbarHostRef = useRef<HTMLDivElement | null>(null); const [toolbarH, setToolbarH] = useState(0);
2. Ensure useLayoutEffect/useRef/useState are imported from react.
3. useLayoutEffect: measure the host via ResizeObserver, guard for SSR (if (!host || typeof ResizeObserver === "undefined") return;), setToolbarH from contentRect.height, ro.observe(host), cleanup with ro.disconnect().
4. Wrap TopChromeBar in <div ref={toolbarHostRef}><TopChromeBar ... /></div>.
5. On the content container below it (the rounded-b-lg ... bg-slate-50 div), add style={{ marginTop: toolbarH }}.

Show me the diff before finalizing. Single file, layout-only.

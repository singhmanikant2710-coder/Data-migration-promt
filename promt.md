
Add a new sidebar navigation link. Do NOT modify anything else.

Open this file: frontend/src/components/Sidebar.tsx

Find this exact array (the first NavSection's items):
items: [
  { label: "Customer CASRR Home", href: "/" },
  { label: "Load Samples", href: "/load-samples" },
  { label: "Review Queue", href: "/review-queue" },
  { label: "Review Status", href: "/review-status" }
]

ADD a new item right after "Review Status", so it becomes:
items: [
  { label: "Customer CASRR Home", href: "/" },
  { label: "Load Samples", href: "/load-samples" },
  { label: "Review Queue", href: "/review-queue" },
  { label: "Review Status", href: "/review-status" },
  { label: "Review History", href: "/review-history" }
]

Do not change any other section (Maintenance, Admin) or any 
other part of this file.

Show me exactly the lines changed.

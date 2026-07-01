# UX-Master

Frontend UX verification skill for AI agents. Validates UI components and pages against a comprehensive UX checklist covering 30 element types including buttons, forms, modals, tables, navigation, accessibility (WCAG 2.2 AA), responsive design, keyboard support, and more.

## Installation

```bash
npx skills install ux-master
```

Or add directly from GitHub:

```bash
npx skills add <your-username>/agent-skills
```

## Usage

Invoke the skill in Claude Code or any compatible agent:

```
/ux-master
```

Or ask naturally:

```
Review this component for UX issues
Verify this form follows UX best practices
Audit the accessibility of this page
```

## What It Checks

- **30 element types**: Button, Link, Input, Textarea, Select, Checkbox, Radio, Switch, Form, Modal, Toast, Tooltip, Tabs, Accordion, Navigation, Table, Pagination, Card, Image, Loading, Empty State, Search, Date Picker, File Upload, and more
- **7 universal criteria**: Recognizability, Operability, Feedback, Consistency, Accessibility, Responsiveness, Error prevention
- **Accessibility (WCAG 2.2 AA)**: Semantic HTML, keyboard navigation, screen reader support, color contrast, focus management
- **Responsive design**: Mobile, tablet, desktop behavior
- **States**: Default, hover, focus, active, disabled, loading, empty, error, success

## Structure

```
ux-master/
├── SKILL.md        # Main skill definition (loaded by AI agents)
├── README.md       # Human-readable usage guide
└── metadata.json   # Package metadata
```

## Publishing

1. Push to a GitHub repository
2. Install via `npx skills add <owner>/<repo>`
3. Or host the file and use `npx skills add <url>`

## License

MIT

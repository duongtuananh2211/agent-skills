# Agent Skills

A collection of AI agent skills for frontend development and UX verification.

## Skills

### ux-master

Frontend UX verification — reviews UI components and pages against a comprehensive checklist covering 30 element types (buttons, forms, modals, navigation, tables, etc.) and 7 universal UX criteria. Targets WCAG 2.2 Level AA compliance.

```bash
npx skills add <owner>/agent-skills --skill ux-master
```

## Structure

```
skills/<name>/
├── SKILL.md        # Skill definition loaded by AI agents
├── README.md       # Human-readable usage guide
└── metadata.json   # Package metadata
```

## Publishing

Push to GitHub, then install anywhere with:

```bash
npx skills add <owner>/agent-skills
```

Skills appear on [skills.sh](https://skills.sh) automatically through install telemetry — no registration needed.

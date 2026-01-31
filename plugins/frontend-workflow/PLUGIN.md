---
name: frontend-workflow
description: Complete frontend development workflow with design skills, linting, and quality hooks
tags: [frontend, workflow, design, linting, quality]
components:
  skills:
    - frontend-design
  agents: []
  rules: []
  verifiers:
    - eslint
  hooks:
    - event: pre-commit
      command: npm run lint
      description: Run linting before commits
    - event: post-file-edit
      command: npm run format -- $FILE
      description: Auto-format files after editing
---

# Frontend Workflow Plugin

A comprehensive frontend development workflow that bundles design skills with quality assurance tools.

## What's Included

### Skills
- **frontend-design**: Production-grade UI design with distinctive aesthetics, modern CSS, and responsive layouts

### Verifiers
- **eslint**: JavaScript/TypeScript linting to catch errors and enforce code style

### Hooks
- **pre-commit**: Automatically runs linting before each commit to prevent broken code
- **post-file-edit**: Auto-formats files after editing to maintain consistent style

## Use Cases

- Starting a new frontend project with best practices baked in
- Teams wanting consistent frontend quality standards
- Projects requiring automated code quality checks

## Installation

```bash
emdash install plugin:frontend-workflow
```

This will install all bundled components and configure the hooks in your project.

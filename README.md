# emdash Registry

Community registry of skills, rules, agents, and verifiers for emdash.

## Structure

```
registry/
├── registry.json          # Index of all components
├── skills/                # Skill definitions
│   └── {name}/SKILL.md
├── rules/                 # Coding rules and standards
│   └── {name}.md
├── agents/                # Custom agent configurations
│   └── {name}.md
└── verifiers/             # Verification configurations
    └── {name}.json
```

## Usage

Use the emdash CLI to browse and install components:

```bash
# List available components
emdash registry list

# Install a skill
emdash registry install skill:frontend-design

# Install a rule
emdash registry install rule:typescript

# Install an agent
emdash registry install agent:reviewer

# Install a verifier
emdash registry install verifier:eslint
```

Or use the interactive wizard:

```bash
emdash registry
```

## Component Formats

### Skills (`skills/{name}/SKILL.md`)

```markdown
---
name: skill-name
description: When to use this skill
user_invocable: true
tools: [tool1, tool2]
---

# Skill Title

Skill instructions...
```

### Rules (`rules/{name}.md`)

Plain markdown with coding guidelines.

### Agents (`agents/{name}.md`)

```markdown
---
description: Agent description
model: claude-sonnet-4-20250514
tools: [grep, glob, read_file]
mcp_servers:
  server-name:
    command: server-command
    args: []
---

# System Prompt

Agent instructions...
```

### Verifiers (`verifiers/{name}.json`)

```json
{
  "type": "command",
  "name": "verifier-name",
  "command": "npm test",
  "timeout": 120,
  "pass_on_exit_0": true,
  "enabled": true
}
```

## Contributing

1. Fork this repository
2. Add your component in the appropriate directory
3. Update `registry.json` with metadata
4. Submit a pull request

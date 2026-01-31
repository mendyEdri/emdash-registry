# Plugin Specification for Emdash Registry

This document describes the plugin format and schema for integrating plugin support into the emdash CLI.

## Overview

Plugins are bundles that combine multiple registry components (skills, rules, agents, verifiers, hooks) into a single installable package. They enable users to install a complete workflow or capability with one command.

## Directory Structure

```
plugins/
└── {plugin-name}/
    ├── PLUGIN.md              # Plugin definition (required)
    ├── skills/                # Embedded skills (optional)
    │   └── {skill-name}/
    │       ├── SKILL.md
    │       └── *.md           # Additional skill files
    ├── rules/                 # Embedded rules (optional)
    │   └── {rule-name}.md
    ├── agents/                # Embedded agents (optional)
    │   └── {agent-name}.md
    └── verifiers/             # Embedded verifiers (optional)
        └── {verifier-name}.json
```

## PLUGIN.md Schema

Plugins use YAML frontmatter with a markdown body.

### Frontmatter Schema

```yaml
---
name: string                    # Required: Plugin identifier (kebab-case)
description: string             # Required: What the plugin does
tags: string[]                  # Required: Categorization tags
components:                     # Required: Components included in the plugin
  skills: string[]              # Optional: Skill names (from registry or embedded)
  rules: string[]               # Optional: Rule names (from registry or embedded)
  agents: string[]              # Optional: Agent names (from registry or embedded)
  verifiers: string[]           # Optional: Verifier names (from registry or embedded)
  hooks: HookDefinition[]       # Optional: Inline hook definitions
---
```

### Hook Definition Schema

```yaml
hooks:
  - event: string               # Required: Hook event type
    command: string             # Required: Command to execute
    description: string         # Optional: What the hook does
    working_dir: string         # Optional: Working directory for command
    timeout: number             # Optional: Timeout in seconds
```

#### Supported Hook Events

| Event | Trigger |
|-------|---------|
| `pre-task` | Before starting a task |
| `post-task` | After completing a task |
| `pre-commit` | Before git commit |
| `post-commit` | After git commit |
| `post-file-edit` | After editing a file |
| `on-correction` | When user provides a correction |
| `on-error` | When an error occurs |

### Example PLUGIN.md

```yaml
---
name: frontend-workflow
description: Complete frontend development workflow with design skills and linting
tags: [frontend, workflow, design, linting]
components:
  skills:
    - frontend-design           # From registry
    - learner                   # Embedded in plugin
  rules:
    - react-best-practices      # From registry
  agents: []
  verifiers:
    - eslint                    # From registry
  hooks:
    - event: pre-commit
      command: npm run lint
      description: Run linting before commits
    - event: post-file-edit
      command: npm run format -- $FILE
      description: Auto-format files after editing
---

# Frontend Workflow Plugin

Documentation in markdown body...
```

## registry.json Schema

Plugins are indexed in `registry.json` under the `plugins` key.

```json
{
  "plugins": {
    "plugin-name": {
      "path": "plugins/plugin-name/PLUGIN.md",
      "description": "Plugin description",
      "tags": ["tag1", "tag2"],
      "components": {
        "skills": ["skill-name"],
        "agents": ["agent-name"],
        "rules": ["rule-name"],
        "verifiers": ["verifier-name"],
        "hooks": 2
      }
    }
  }
}
```

### Component Resolution

Components can be:
1. **Registry references**: Names of components in the main registry
2. **Embedded**: Components defined within the plugin directory

Resolution order:
1. Check if component exists in plugin's embedded directories
2. Fall back to main registry
3. Error if not found in either

## Installation Flow

When a user runs `emdash install plugin:{name}`:

```
1. Fetch plugin metadata from registry.json
2. Download PLUGIN.md and parse frontmatter
3. For each component type:
   a. Resolve component (embedded vs registry)
   b. Download component files
   c. Install to user's project
4. Register hooks in project config
5. Report installed components
```

### Suggested Installation Structure

```
.emdash/
├── config.json                 # Emdash configuration
├── plugins/
│   └── {plugin-name}/          # Installed plugin
│       ├── PLUGIN.md
│       └── skills/
│           └── {skill}/
├── hooks/                      # Active hooks
│   └── post-task.json
└── learnings/                  # For self-improving plugin
    ├── semantic.md
    ├── procedural.md
    └── episodes.md
```

## Component Schemas

### Skill Schema (SKILL.md)

```yaml
---
name: string                    # Required: Skill identifier
description: string             # Required: When/why to use this skill
user_invocable: boolean         # Required: Can users invoke directly?
tools: string[]                 # Required: Available tools (can be empty)
tags: string[]                  # Optional: Categorization tags
---

# Skill instructions in markdown...
```

Skills can have additional `.md` files in their directory for:
- Memory/learning storage
- Reference documentation
- Templates

### Rule Schema ({name}.md)

Plain markdown files with coding guidelines. No frontmatter required.

```markdown
# Rule Title

Guidelines and standards in markdown...
```

### Agent Schema ({name}.md)

```yaml
---
description: string             # Required: Agent purpose
model: string                   # Optional: LLM model (default: claude-sonnet-4-20250514)
tools: string[]                 # Required: Available tools
mcp_servers:                    # Optional: MCP server configurations
  {server-name}:
    command: string
    args: string[]
---

# System prompt in markdown...
```

### Verifier Schema ({name}.json)

```json
{
  "type": "command",
  "name": "verifier-name",
  "description": "What this verifies",
  "command": "command to run",
  "timeout": 120,
  "pass_on_exit_0": true,
  "enabled": true,
  "tags": ["tag1", "tag2"]
}
```

## Self-Improving Plugin Pattern

The `self-improving` plugin demonstrates an advanced pattern where:

1. **Learner skill** instructs the LLM how to extract learnings
2. **Memory files** (`.md`) store atomic learnings
3. **Hooks** trigger learning extraction automatically
4. **Memory types** separate facts, patterns, and episodes

### Memory File Locations

When installed, learning files should be stored in:
- **Project-level**: `.emdash/learnings/` (codebase-specific)
- **User-level**: `~/.emdash/learnings/` (cross-project preferences)

### Learning Format

```markdown
- [YYYY-MM-DD] Atomic learning content
- [SUPERSEDED by YYYY-MM-DD] Old learning (replaced)
```

## CLI Commands (Suggested)

```bash
# Install a plugin
emdash install plugin:{name}

# List installed plugins
emdash plugins list

# Show plugin details
emdash plugins info {name}

# Uninstall a plugin
emdash uninstall plugin:{name}

# Invoke a skill from a plugin
emdash skill {skill-name}

# Update plugins from registry
emdash plugins update
```

## Versioning

Plugins inherit the registry version. Future consideration:
- Add `version` field to plugin frontmatter
- Add `min_emdash_version` for compatibility checks
- Add `dependencies` for plugin-to-plugin dependencies

## Example Plugins

### 1. frontend-workflow
Bundles: frontend-design skill + eslint verifier + formatting hooks

### 2. self-improving
Bundles: learner skill + post-task/on-correction hooks + memory files

### 3. python-tdd (hypothetical)
Bundles: pytest verifier + type-check verifier + pre-commit hooks + tdd-workflow rule

## Implementation Notes for Emdash CLI

1. **Parsing**: Use a YAML parser for frontmatter extraction
2. **Hook registration**: Store hooks in `.emdash/hooks/` or config.json
3. **Component deduplication**: If same component referenced by multiple plugins, install once
4. **Offline support**: Consider caching downloaded plugins
5. **Integrity**: Consider adding checksums to registry.json
6. **Updates**: Track installed versions to enable updates

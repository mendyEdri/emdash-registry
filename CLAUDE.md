# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the **emdash Registry** - a community registry of reusable components for the emdash CLI tool. It's a configuration/documentation repository (no runtime code) that catalogs:
- **Skills**: Reusable AI competencies
- **Rules**: Coding standards and guidelines
- **Agents**: Custom AI agent configurations
- **Verifiers**: Command-based validation configs

## Repository Structure

- `registry.json` - Central index of all components with metadata
- `skills/{name}/SKILL.md` - Skill definitions with YAML frontmatter
- `rules/{name}.md` - Plain markdown coding guidelines
- `agents/{name}.md` - Agent configs with YAML frontmatter + system prompt
- `verifiers/{name}.json` - Verification command configurations

## Component Formats

**Skills** use YAML frontmatter with `name`, `description`, `user_invocable`, `tools`, and markdown instructions below.

**Agents** use YAML frontmatter with `description`, `model`, `tools`, optional `mcp_servers`, and system prompt in markdown body.

**Verifiers** are JSON with `type`, `name`, `command`, `timeout`, `pass_on_exit_0`, `enabled`, and `tags`.

## Working With This Repository

When adding new components:
1. Create the component file in the appropriate directory following existing formats
2. Update `registry.json` with the component's metadata (path, description, tags)

All agents use model `claude-sonnet-4-20250514` by default.

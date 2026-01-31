---
name: self-improving
description: A plugin that learns from interactions and improves over time by extracting and persisting knowledge about user preferences, codebase patterns, and outcomes
tags: [memory, learning, self-improving, productivity, personalization]
components:
  skills:
    - learner
  agents: []
  rules:
    - use-learnings
  verifiers:
    - learning-validator
  hooks:
    - event: post-task
      command: emdash skill learner --mode=auto
      description: Automatically extract learnings after completing tasks
    - event: on-correction
      command: emdash skill learner --mode=correction
      description: Capture learnings when user provides corrections
---

# Self-Improving Plugin

A plugin that enables your AI assistant to learn and improve over time by extracting and persisting knowledge from interactions.

## How It Works

```
┌─────────────────────────────────────────────────────────┐
│                    Interaction                          │
│   (task completion, correction, failure, success)       │
└─────────────────────┬───────────────────────────────────┘
                      │
                      ▼
┌─────────────────────────────────────────────────────────┐
│                  Learner Skill                          │
│   - Analyzes context                                    │
│   - Extracts meaningful patterns                        │
│   - Generalizes to principles                           │
│   - Checks for contradictions                           │
└─────────────────────┬───────────────────────────────────┘
                      │
          ┌───────────┼───────────┐
          ▼           ▼           ▼
    ┌──────────┐ ┌──────────┐ ┌──────────┐
    │ semantic │ │procedural│ │ episodes │
    │   .md    │ │   .md    │ │   .md    │
    └──────────┘ └──────────┘ └──────────┘
         │            │            │
         └────────────┼────────────┘
                      ▼
┌─────────────────────────────────────────────────────────┐
│              Future Interactions                        │
│   Learnings loaded as context → Better assistance       │
└─────────────────────────────────────────────────────────┘
```

## Memory Types

### Semantic Memory (`semantic.md`)
Generalized facts that always apply:
- User preferences: "Prefers TypeScript over JavaScript"
- Codebase facts: "Uses pnpm, not npm"
- Domain knowledge: "A 'widget' in this codebase means X"

### Procedural Memory (`procedural.md`)
Conditional action patterns:
- "When creating components, always add tests"
- "Before database changes, check migrations"
- "Never commit without running linter"

### Episodic Memory (`episodes.md`)
Notable interactions with outcomes:
- Situation → Action → Result → Lesson
- Tracks both successes and failures
- Prevents repeating mistakes

## What Gets Learned

| Trigger | Example | Memory Type |
|---------|---------|-------------|
| User correction | "No, use tabs not spaces" | Semantic |
| Explicit preference | "I always want verbose logging" | Semantic |
| Task success | Approach that worked well | Procedural |
| Task failure | Approach that didn't work | Episode |
| Codebase discovery | "This project uses monorepo" | Semantic |

## Rule: use-learnings

Always loaded into context. Ensures the assistant:
- Consults learnings before starting tasks
- Applies user preferences consistently
- Avoids repeating documented mistakes
- Follows established patterns

## Verifier: learning-validator

An LLM-based verifier that validates learnings before they're saved:
- **Actionable**: Can it be applied to future situations?
- **Specific**: Is it useful (not vague)?
- **General**: Does it apply beyond one situation?
- **Non-duplicate**: Doesn't already exist?
- **Non-contradictory**: Doesn't conflict without superseding?

Returns: `PASS`, `FAIL <reason>`, or `SUPERSEDE <line>`

## Hooks

### `post-task`
After completing any significant task, automatically invokes the learner to extract potential learnings.

### `on-correction`
When the user corrects the assistant, captures the correction as a learning to prevent future mistakes.

## Usage

### Automatic Learning
With hooks enabled, learning happens automatically after tasks and corrections.

### Manual Learning
Invoke the learner skill directly:
```bash
emdash skill learner
```

### Review Learnings
All learnings are stored in plain markdown files within the skill directory:
- `skills/learner/semantic.md`
- `skills/learner/procedural.md`
- `skills/learner/episodes.md`

You can read, edit, or delete any learning directly.

## Installation

```bash
emdash install plugin:self-improving
```

## Design Principles

1. **LLM-driven**: The AI extracts learnings, no scripts
2. **Atomic**: One learning per line, dated
3. **Human-readable**: Plain markdown, easily editable
4. **Generalized**: Principles over specifics
5. **Contradiction-aware**: Old learnings superseded, not deleted
6. **Quality over quantity**: Only meaningful learnings

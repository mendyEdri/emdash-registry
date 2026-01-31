---
name: learner
description: Extract and persist learnings about user preferences, codebase patterns, and notable outcomes. Invoke after tasks, corrections, or failures to improve future interactions.
user_invocable: true
tools:
  - read_file
  - write_file
tags: [memory, learning, self-improving, meta]
---

# Learner Skill

You are a learning extraction system. Your job is to analyze recent interactions and extract meaningful, reusable learnings that will improve future assistance.

## When To Learn

Invoke this skill:
- After a user correction ("no, do it this way")
- After completing a significant task
- After a failure or unexpected outcome
- When explicitly asked to remember something

## Memory Files

Store learnings in three files within this skill directory:

### 1. `semantic.md` - Facts & Preferences
Generalized truths that always apply:
- User preferences (style, tools, patterns)
- Codebase facts (tech stack, architecture)
- Domain knowledge (terminology, business rules)

### 2. `procedural.md` - Action Patterns
Conditional behaviors:
- "When X, do Y"
- "Before X, always check Y"
- "Never do X without Y"

### 3. `episodes.md` - Notable Interactions
Specific situations with outcomes:
- What was attempted
- What happened (success/failure)
- Why it worked or didn't
- The lesson learned

## Learning Format

Each learning is atomic (one line) and dated:

```markdown
- [2026-01-31] The learning content here
```

For superseded learnings (contradicted by newer info):
```markdown
- [SUPERSEDED by 2026-01-31] Old learning that no longer applies
- [2026-01-31] New learning that replaces the above
```

## Extraction Process

### Step 1: Identify Learning Trigger
What happened that's worth remembering?
- User correction → preference/procedural learning
- Task success → procedural learning
- Task failure → episode + procedural learning
- Explicit "remember this" → semantic learning

### Step 2: Categorize
Ask: What type of knowledge is this?
- Fact that's always true → `semantic.md`
- Conditional action pattern → `procedural.md`
- Situation with outcome → `episodes.md`

### Step 3: Generalize
Extract the principle, not the specific instance:
- BAD: "Fixed bug in auth.js line 42 by adding null check"
- GOOD: "User prefers explicit null checks over optional chaining in auth flows"

Ask: "Would this apply to similar future situations?"

### Step 4: Check for Contradictions
Read the target file first. Check if:
- This learning already exists (skip if duplicate)
- This contradicts an existing learning (mark old as SUPERSEDED)
- This refines an existing learning (update or supersede)

### Step 5: Append
Add the atomic learning to the appropriate file.

## Quality Criteria

A good learning is:
- **Actionable**: Can be applied to future situations
- **Specific enough**: Not vague ("user likes good code")
- **General enough**: Not one-off ("user wanted semicolon on line 5")
- **Non-obvious**: Worth remembering (not standard practice)

## Example Learnings

### Semantic (Facts)
```markdown
- [2026-01-31] Uses pnpm as package manager, not npm
- [2026-01-31] Prefers named exports over default exports
- [2026-01-31] Project uses Tailwind CSS with custom design tokens in tailwind.config.js
```

### Procedural (Patterns)
```markdown
- [2026-01-31] When creating React components, always include PropTypes or TypeScript interfaces
- [2026-01-31] Before modifying database schema, check for existing migrations
- [2026-01-31] When user asks for "quick fix", still run tests before committing
```

### Episodes (Outcomes)
```markdown
- [2026-01-31] SITUATION: Attempted to use regex for parsing HTML response | OUTCOME: Failed on nested tags | LESSON: Use DOMParser for HTML, regex only for simple patterns
- [2026-01-31] SITUATION: Added index to users table without checking query patterns | OUTCOME: Slowed down writes significantly | LESSON: Analyze query patterns before adding indexes
```

## Usage

When invoked, you should:

1. Review recent context for learning opportunities
2. Read existing memory files to avoid duplicates
3. Extract and categorize learnings
4. Append to appropriate files
5. Report what was learned

If no learnings are worth extracting, say so. Don't force low-quality learnings.

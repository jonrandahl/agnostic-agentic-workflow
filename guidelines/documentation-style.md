# Documentation Style Guide — Guideline File Standards

This file documents how to write guideline files for the shared agentic system.
Use this when creating or updating files in ~/.agents/guidelines/.

---

## Purpose

Guideline files must be readable for humans first, executable for agents second.
When you return to a file six months from now, the structure should tell you why
it exists, not just what the rules are.

---

## Why Structure Matters

- **Humans need context.** A list of rules without explanation is a reference
  manual, not guidance.
- **Maintenance requires clarity.** Future updates are easier when you
  understand the original intent.
- **Agents benefit from clarity too.** Clear structure helps agents parse and
  apply rules correctly.
- **Shared rules stay stable.** When rules are well-explained, they are less
  likely to be duplicated elsewhere.

---

## Recommended Structure

Every guideline file should follow this pattern:

### 1. Title and Opening

```
# Guideline Name — One-Line Purpose

One or two sentences explaining what this file is for and who it applies to.
```

Example: "# Core Guidelines — Universal for All Agents"

### 2. Purpose Section

Explain why this guideline exists. What problem does it solve?

```markdown
## Purpose

Brief explanation of why these rules exist.
Who benefits? What is the outcome?
```

Example: "Prevent unrecorded progress by prompting for optional commit
checkpoints…"

### 3. Context Section (optional but recommended)

Explain when and how this guideline applies. This is critical for human readers.

```markdown
## Why This Matters

- Describe the problem being addressed
- Give examples of failure modes
- Explain the philosophy
- Note any edge cases
```

### 4. Core Content Sections

Organised by topic. Use consistent hierarchy:

- Level 1 (`##`): Major topic areas
- Level 2 (`###`): Sub-topics
- Level 3 (`####`): Implementation detail (rarely needed)

Use bullets for rules. Use prose for explanation.

```markdown
## Topic Name

Introductory prose explaining the topic.

### Sub-topic

- Rule format: concise, actionable
- Each bullet is one atomic rule
- Do not mix bullets with prose within a section
```

### 5. Examples and Code Blocks

- Use code blocks for real examples
- Keep examples concise but complete
- Always provide at least one positive example
- Optional: show a negative example if the error is common

```markdown
### File header

Every JavaScript file should open with a file overview:

```js
/**
 * @fileOverview Brief description of the file's purpose.
 * @see https://link-to-relevant-spec-or-reference.com
 */
```

Good: Clear, minimal, includes @fileOverview and @see tags.

```

### 6. Boundary Section (if applicable)

Clarify what this file owns and what it does not.

```markdown
## What This File Owns

- Rules that apply universally
- Patterns that affect multiple projects

## What Belongs Elsewhere

- Project-specific rules: put in project-level CLAUDE.md
- Agent-specific rules: put in ~/.agents/agent-adapters/{agent}/adapter.md
- Temporary guidelines: use project README or decision log
```

### 7. Maintenance Section

Always document how to maintain this file.

```markdown
## Maintenance

**Universal change?** Update this file.
**Agent-specific rule?** Put it in the agent adapter.
**Project-specific rule?** Put it in the project CLAUDE.md.

Avoid duplication. If you find yourself copying text from another file,
that is a sign the rule should stay in the shared file.
```

### 8. Footer

```markdown
*Last updated: May 2026*
*Shared guidelines location: ~/.agents/guidelines/*
*Related files: [list any cross-references]*
```

---

## Formatting Rules

### Headers

- Use sentence case for all headers
- Level 1 (`#`) for document title
- Level 2 (`##`) for major sections
- Level 3 (`###`) rarely needed

### Prose vs. Bullets

- **Use prose** for explanation, context, philosophy
- **Use bullets** for rules, requirements, patterns
- **Never mix** prose and bullets in the same section

### Lists

- Use `-` for bullet lists (not `*` or `+`)
- Use descriptive list items (not single words)
- Oxford comma in lists of three or more items

### Emphasis

- Use `**bold**` sparingly, only for key terms or callouts
- Use `_italic_` for emphasis within prose
- Never use all caps for emphasis
- Never use multiple levels of emphasis in one sentence

### Code

- Use triple backticks for code blocks, not inline code for long examples
- Specify language: ` ```js ` `,` ```css ` `, etc.
- Keep examples under 10 lines where possible
- Always show complete, working examples

---

## What NOT To Do

- **Do not** duplicate rules that already exist in another shared file
- **Do not** include temporary or experimental rules
- **Do not** use this file to document implementation detail (comments belong in
  code)
- **Do not** write rules for one-off exceptions (those belong in project-level
  files)
- **Do not** assume agents know project-specific context
- **Do not** bury important information in subsections

---

## Migration Example

Before (rule-only):

```
# Commit Rules

- Always use Conventional Commits: type(scope): subject
- Allowed types: feat, fix, docs, style, refactor, test, chore, perf, ci, build
```

After (with structure):

```
# Version Control Conventions

## Purpose

Standardised commit messages make it easy to scan history, automate tooling,
and understand intent at a glance.

## Commit Format

Always use Conventional Commits. This is the industry standard format.

### Structure

```

type(scope): subject

body (optional)

```

### Allowed Types

- `feat` – new feature
- `fix` – bug fix
- `docs` – documentation only
- `style` – formatting, no code change
- `refactor` – code restructure, no behaviour change
- `test` – test additions or changes
- `chore` – tooling, dependencies, CI/CD
- `perf` – performance improvement
- `ci` – CI/CD configuration
- `build` – build system changes

### Subject Line

- Imperative mood ("add feature", not "added feature")
- 50 characters maximum
- No full stop at the end
- British English spelling
```

---

## Maintenance of This File

When you create a new guideline file, reference this file. When you update an
existing guideline file, check that it still follows this structure. When you
find yourself breaking these rules, consider why — the exception might reveal a
better structure.

---

*Last updated: 18 May 2026* *Shared guidelines location: ~/.agents/guidelines/*
*Related files: core.md, technical-patterns.md, commit-checkpoint-guideline.md*

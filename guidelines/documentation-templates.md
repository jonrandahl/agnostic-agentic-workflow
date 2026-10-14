# Documentation Templates and Patterns

## Overview

This document captures reusable documentation patterns observed across
successful projects (specifically prometheus-metrics). Use these as templates
for new projects to avoid re-discovering documentation structure each time.

## Core Documentation Files

### README.md

**Purpose**: Entry point for new users and developers.

**Structure**:

1. Title (one-liner mission statement)
2. Table of Contents with anchors (for long READMEs)
3. Requirements section (versions, dependencies)
4. Installation (step-by-step, with example commands)
5. Quick start (copy-paste commands to get running)
6. Core concepts (explain the "why" and "what")
7. Configuration section (with defaults)
8. Common tasks / examples
9. Troubleshooting / verification steps
10. Links to deeper documentation

**Key techniques**:

- Lead with mission, not history (avoid "This started as a CodePen")
- Use a TOC for documents longer than ~500 lines
- Provide copy-paste commands (exact versions matter)
- Show migration paths for existing projects

### CONTRIBUTING.md

**Purpose**: Onboarding for contributors and developers.

**Structure**:

1. Development setup (prerequisites, install commands)
2. Available commands (make targets, npm scripts, etc.)
3. Code style conventions (language, formatting, naming)
4. How to add features (with a concrete example: "adding a subscriber")
5. Test conventions (isolation patterns, fixtures, mocks)
6. Pre-commit checklist (what must pass before PR)
7. How to submit changes (PR discipline, issue-first rule)

**Key techniques**:

- Copy-paste setup commands (no hand-waving)
- Test isolation patterns as reusable code templates
- Concrete feature-adding example (not abstract)
- Clear PR discipline (open issue first, focused PRs, changelog required)

### CHANGELOG.md

**Purpose**: User-facing release notes and historical record.

**Standard**:

- Use [Keep a Changelog](https://keepachangelog.com/) format
- Combine with [Conventional Commits](https://www.conventionalcommits.org/) if
  applicable
- Adhere to [Semantic Versioning](https://semver.org/)

**Structure**:

- `## [Unreleased]` section at top for in-progress work
- Sections: Added, Changed, Deprecated, Removed, Fixed, Security
- One markdown link at the bottom per version: `[version]:
  https://github.com/...compare/v1.0.0...v1.1.0`
- Date format: `YYYY-MM` (not full dates, balances precision vs brittleness)

**Key techniques**:

- Describe the **action taken**, not the effect (e.g. "Migrated from npm to
  pnpm" not "Improved build speed")
- Avoid file paths and implementation details
- Map Conventional Commit types to KaC sections: feat→Added, fix→Fixed,
  perf/refactor→Changed
- All changes in Unreleased until a version is tagged

### SECURITY.md (optional for public projects)

**Purpose**: Define vulnerability reporting process and scope.

**Structure**:

1. Supported versions (which versions receive patches)
2. Reporting procedure (email, SLA, credit option)
3. In-scope vs out-of-scope vulnerabilities (be explicit)
4. Timeline expectations (acknowledgement, resolution)

**Key techniques**:

- Define **explicit boundaries** on what is/isn't a vulnerability
- Provide private email for reports (not public GitHub issues)
- Set clear SLAs (e.g. 5 days acknowledgement, 30 days resolution)
- Credit reporters unless they opt out

### AGENTS.md (context-dependent, not required for all projects)

**Purpose**: High-level overview for AI agents and developers.

**When to create**:

- Project has non-obvious file structure or patterns (e.g. generators,
  middleware setup)
- Known coverage gaps, intentional uncovered code, or excluded test suites
- Complex configuration with defaults that are not obvious from README
- Project-specific state that agents need to understand (e.g. "railtie is
  intentionally uncovered")
- Deeper documentation exists (e.g. `docs/prometheus-rails-implementation.md`)
  that agents should reference

**When to skip**:

- Project is straightforward (simple file structure, no generators, no coverage
  gaps)
- All important information is already in README and CONTRIBUTING
- No project-specific state to document
- Unified ~/.agents/ guidelines provide sufficient context

**Structure** (if created):

1. Project mission (one-liner)
2. Quick start with available commands
3. Current state and known issues (transparency)
4. File tree / project structure
5. Configuration with examples
6. Links to deeper documentation

**Key techniques**:

- Transparency on known gaps and issues (intentional uncovered code, excluded
  generators)
- Visual file tree helps orient agents to codebase
- Configuration defaults with real examples
- Delegate technical details to deeper docs

### CLAUDE.md (context-dependent, not required for all projects)

**Purpose**: Entry point for Claude/Copilot agents.

**When to create**:

- Only if AGENTS.md exists (CLAUDE.md delegates to it)
- Not necessary if shared ~/.agents/ guidelines provide sufficient context

**When to skip**:

- Project doesn't require AGENTS.md
- Unified agentic setup at ~/.agents/ is the source of truth for agent
  instructions

**Standard** (if created):

- Minimal (1-2 lines)
- Delegate to AGENTS.md for full context
- Example: "See AGENTS.md for full project context."

---

## Decision: When to Include AGENTS.md and CLAUDE.md

With a unified agentic approach in ~/.agents/, project-specific AGENTS.md and
CLAUDE.md are **optional**:

**Include them if**:

- Project has generators, build complexity, or non-obvious patterns
- There are known coverage gaps or intentional uncovered code to document
- Agents need project-specific context beyond what's in README/CONTRIBUTING
- You want transparent documentation of "current state" and "known issues"

**Skip them if**:

- Project structure is straightforward and discoverable
- All critical information is in README and CONTRIBUTING
- Agents can discover everything from the file system
- Shared ~/.agents/ guidelines provide sufficient context

**Example decision**:

- `prometheus-metrics` (Rails gem): Include AGENTS.md (generators, coverage
  gaps, config defaults)
- `wcag-contrast-checker` (frontend web app): Skip AGENTS.md (straightforward
  structure, no hidden state)

---

## Mapping to Project Types

### Rails Gem Projects

- README: installation via Gemfile, configuration, examples
- CONTRIBUTING: make targets, test conventions, test fixtures
- CHANGELOG: Keep a Changelog + Conventional Commits (required)
- SECURITY: vulnerability scope (recommended for public gems)
- AGENTS: include (generators, coverage gaps, config defaults warrant
  documentation)

### Frontend Web Apps

- README: quick start, theme modes, commands, testing (required)
- CONTRIBUTING: pnpm/npm commands, file structure, code style, testing
  (required)
- CHANGELOG: Keep a Changelog + Conventional Commits (required)
- SECURITY: optional (depends on public exposure)
- AGENTS: skip (straightforward structure; README/CONTRIBUTING suffice)

### Node.js Libraries

- README: installation via npm, quick start, API docs link (required)
- CONTRIBUTING: npm run commands, test conventions (required)
- CHANGELOG: Keep a Changelog + Conventional Commits (required)
- SECURITY: optional
- AGENTS: skip unless project has complex patterns or generators

---

## British English Standards

Use throughout all documentation:

- colour (not color)
- organise (not organize)
- customise (not customize)
- behaviour (not behavior)
- centre (not center)

---

## Links and References

- [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)
- [Semantic Versioning](https://semver.org/spec/v2.0.0.html)

## Live Examples

- Rails gem: `<local-project-root>/`
  - Comprehensive README with TOC, migration guide, configuration
  - CONTRIBUTING with make targets and test patterns
  - CHANGELOG with Conventional Commits mapping
  - SECURITY with explicit scope boundaries
  - AGENTS.md with transparent current state

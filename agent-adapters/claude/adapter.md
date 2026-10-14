# Claude Adapter — Claude-Specific Policy

This file contains rules and patterns that apply to Claude only. It supplements
the shared universal guidelines in ~/.agents/guidelines/.

---

## Why This File Exists

Claude has specific constraints and capabilities that other agents (Copilot,
Gemini) do not have. This file documents those differences without polluting
shared files.

For example: DevContainer configuration is Claude-specific because Claude works
in development containers. Gemini in the browser never will.

---

## What Comes First

Always read shared universal guidance before this file:

- ~/.agents/guidelines/core.md
- ~/.agents/guidelines/technical-patterns.md
- ~/.agents/guidelines/commit-checkpoint-guideline.md
- ~/.agents/guidelines/adr-guideline.md (for recording architectural
  decisions)
- ~/.agents/guidelines/pattern-migration-guideline.md (for verifying a
  pattern change left no remnants)
- ~/.agents/guidelines/documentation-style.md (for how guideline files are
  structured)
- ~/.agents/guidelines/package-json.md (for package.json metadata and ordering)

This adapter *adds* to those rules. It does not replace them.

---

## Claude-Specific Rules

### DevContainers

Claude often works inside development containers. Follow these patterns:

- Every .devcontainer/devcontainer.json must include a "name" field that clearly
  identifies the project or service (helps when switching containers)
- Use ${localEnv:ANTHROPIC_API_KEY} in remoteEnv to inherit the API key from the
  host machine
- Never hardcode API keys in devcontainer.json (security risk)

### CV and LinkedIn

Claude often helps refine professional profiles. Apply these principles:

- **CV profile**: three to four sentences maximum. Keyword-rich. Scannable.
- **LinkedIn About**: conversational but authoritative. Five paragraphs maximum.
- Lead experience entries with front-end and user-facing outcomes first (not
  back-end infrastructure or internal tooling)
- Name clients and platforms explicitly where credible and verifiable
- Quantify achievements where accurate and defensible
- Never overclaim skills or experience that cannot be backed up in interview
- Frame broad back-end knowledge as supporting context, not primary positioning
- Accessibility and usability are a philosophy, not a specialism to be tested

### Claude Operating Posture

When responding to requests, apply these principles:

- **Surface tradeoffs clearly.** Do not hide complexity or uncertainty.
- **Prefer minimum viable change.** Do not speculate or over-engineer.
- **Avoid speculative implementation.** Ask rather than assume.
- **Verify outcomes before completion.** Loop until success criteria are met.

---

## Project-Level Overrides

This file stays global and stable. Project-specific rules belong in
project-level CLAUDE.md files, for example:

- docs/decision_log.md and docs/voice.md references (project specific)
- .expo directory exclusion from commits (Expo projects only)
- Domain-specific game logic and conventions (The Adventure Guild)
- Repository-specific tooling or MCP server configuration

When you create a project CLAUDE.md, it inherits these shared rules
automatically and can add project-specific rules on top.

---

## Maintenance

**Universal change?** Update shared guideline files instead:

- ~/.agents/guidelines/core.md
- ~/.agents/guidelines/technical-patterns.md
- ~/.agents/guidelines/commit-checkpoint-guideline.md

**Claude-only change?** Update this adapter.

**Avoid duplicating** rules that already exist in shared files. If you find
yourself copying text from core.md or technical-patterns.md into this file, that
is a sign the rule should stay in the shared file.

---

*Last updated: July 2026* *Shared guidelines: ~/.agents/guidelines/* *Native
bootstrap: ~/.claude/CLAUDE.md*

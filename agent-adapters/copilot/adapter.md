# Copilot Adapter — Shared Guidelines Integration

This file points VS Code Copilot to shared agentic guidelines in ~/.agents.

---

## Shared Guidelines Location

Shared guidelines are maintained in `~/.agents/guidelines/`:

- **Core guidelines**: `~/.agents/guidelines/core.md`
  - Identity & Positioning
  - Tone & Voice
  - Language & Grammar
  - Punctuation & Formatting
  - Structure & Length
  - Version Control
  - AI-Assisted Development
  - External Links

- **Technical patterns**: `~/.agents/guidelines/technical-patterns.md`
  - Dark Theme Implementation
  - JavaScript Documentation (JSDoc)

- **Commit checkpoints**: `~/.agents/guidelines/commit-checkpoint-guideline.md`
  - Agentic workflow for tracking and committing progress
- **Action authorisation**: `~/.agents/guidelines/action-authorisation.md`
  - Risk levels, least-destructive defaults, and approval boundaries

- **Architectural Decision Records**: `~/.agents/guidelines/adr-guideline.md`
  - When a decision warrants an ADR, where it lives, and the template to use

- **Pattern migration verification**: `~/.agents/guidelines/pattern-migration-guideline.md`
  - Search the whole codebase before and after a pattern change; never
    claim "fully migrated" without that search backing it

- **Documentation style**: `~/.agents/guidelines/documentation-style.md`
  - How to structure guideline files
  - Formatting conventions for human readability
  - Maintenance patterns for shared files

- **Package JSON**: `~/.agents/guidelines/package-json.md`
  - Package metadata ordering and required fields
  - Linkable fields such as homepage, repository, and bugs
  - Licence and packageManager guidance for pnpm-based projects

**Apply relevant shared guidelines to Copilot work in VS Code, following the
user's task-specific preferences and project conventions.**

---

## Copilot-Specific Notes

Copilot in VS Code operates within the editor context and has access to:

- Project files and directory structure
- Git status and repository information
- File contents via language server protocol
- VS Code command palette and extension ecosystem

When applying shared guidelines, Copilot should:

1. Follow commit checkpoint guidelines before significant changes
2. Use British English spelling and imperative mood for commits
3. Apply version control rules consistently with other agents
4. Maintain project-specific voice when established
5. Propose an ADR when a decision is hard to reverse or constrains future
   work
6. Apply the shared action authorisation policy before mutating files or using
   external tools
7. Before calling a pattern change complete, search the whole codebase for
   remaining instances of the old pattern, not just the file being edited

---

## How to Integrate This File

1. Keep this file at `~/.agents/agent-adapters/copilot/adapter.md`
2. Reference in VS Code Copilot instructions as the entry point to shared
   guidelines
3. Point Copilot to `~/.agents/guidelines/` for full rule details
4. Add Copilot-specific extensions or VS Code customisations here if needed

---

## How to Update Guidelines

When modifying or creating guideline files, follow the structure documented in
`~/.agents/guidelines/documentation-style.md`. This ensures that future updates
are consistent and that the rationale behind each rule remains clear.

---

*Last updated: 15 July 2026* *Shared guidelines location: ~/.agents/guidelines/*
*Documentation style: ~/.agents/guidelines/documentation-style.md*
